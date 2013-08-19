---
layout: post
title: "Nutch"
categories: [tool, crawler]
---

##名词解释

    CrawlDatum: url的元信息，包括状态和配置等，包括爬取状态，上次爬取的时间，爬取间隔...
    CrawlDB: 存储爬虫已知的所有URL及其元信息。存储格式:<Text, CrawlDatum>。其中的key表示该URL，而value则是CrawlDatum.
    LinkDB: 存放url之间的关系
    Segments: 存放抓取列表以及抓取回来的网页，页面内容有二进制的raw content也有parsed content，nutch也广度优先爬取策略，没爬取一轮生成一个以时间命名的Segment文件夹 
    Index:


##过程

   Inject-->Generate-->Fetch-->Parse-->Update CrawlDB

* Injector: 将目标url注入到CrawlDB中，在爬虫启动前执行，主要工作：  

    1. 把文本文件中的URL进行过滤，规范化，包装成“CrawlDatum”，并最终存成临时的Sequence类型文件。该临时文件的格式为<Key, CrawlDatum>。
    2. 把排序后的结果和原“CrawlDb”中的数据(如果存在)进行合并，生成新的CrawlDb文件。

    * 不知道为什么分成两个MRjob（SortJob&MergeJob），直接一个job不行么？

* Generator: 根据CrawlDB创建抓取列表fetchlist

* Fetcher: 抓取fetchlist中的url

* Parser: 解析抓取的data
  
    // 使用ParseUtil的parse方法解析content
    parseResult = new ParseUtil(getConf()).parse(content); 
      ...
      // ParseUtil首先根据content的类型通过parserFactory得到适用的解析器插件列表
      parsers = this.parserFactory.getParsers(content.getContentType(),
                content.getUrl() != null ? content.getUrl() : "");
        ...
        List<Extension> parserExts = null;
        parserExts = getExtensions(contentType);
          ...
          ObjectCache objectCache = ObjectCache.get(conf);
          List<Extension> extensions = (List<Extension>) objectCache
              .getObject(type);
        
      // 逐一使用解析器，直到成功解析
      ParseResult parseResult = null;
      for (int i = 0; i < parsers.length; i++) {
        if (LOG.isDebugEnabled()) {
          LOG.debug("Parsing [" + content.getUrl() + "] with ["
              + parsers[i] + "]");
        }
        if (maxParseTime != -1)
          parseResult = runParser(parsers[i], content);
        else
          parseResult = parsers[i].getParse(content);

        if (parseResult != null && !parseResult.isEmpty())
          return parseResult;
      }

##配置

1. 基本配置文件
  * nutch-default.xml
  * nutch-site.xml

2. 常见配置
* nutch 默认的下载文件大小上限是65536字节(`nutch-default.xml``ftp.content.limit`配置)，达到后智能截断，可以在`nutch-site.xml`中重写

