---
layout: post
title: "字符集编码问题"
categories: [skill, charset, encoding]
---

### Python中的编码

`Python 2`中，基本字符串(`string`)类型是由`8 bit`的字符组成，为了满足国际化的需求，设计支持`unicode`字符集，从而添加了另外一种字符串类型`unicode string`(在引号前加一个`u`标识)

`unicode string`本身’无编码‘，只是定义了包含的unicode字符序列（比如在unicode字符集中的位置），但是`unicode string`也要存储，就必须涉及以何种编码方式存储，但这个是完全对用户透明的，用户无法知道，也不必知道。

由于unicode字符集是其他字符集(ASCII, GBK, BIG5等）的合集，任何其他字符集都可以用unicode表示（只需要定义一种映射规则而已），而`unicode string`则是定义在`unicode`字符集上的字符表示形式，具有很强的跨平台性，其他编码都可以通过unicode方便的转化，所以很多语言都使用unicode字符集作为默认字符集，如Java和python

定义一个`unicdoe`字符串，`u_str=u'中文'`，如果直接输出到文件`f.write(u_str)`，会报错，因为没有指定'u_str'的编码，python会使用默认的’ascii‘编码该unicode串，遇到非ascii的字符，就会报错：

> UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)

我们知道unicode字符串中含有中文，可以选择诸如`utf-8, utf-16, gbk, gb2312`等含有（简体）中文的字符集（编码方式），如`f.write(u_str.encode('utf-8'))` 或 `f.write(u_str.encode('gbk')`

对于基本字符串，创建时其编码就确定了，一般为平台默认的编码，如中文windows默认是gbk，英文Linux默认是utf-8，

以中文Windows系统为例，假如默认编码是`gbk`，创建字符串`str='中文'`后，`str`已经是`gbk`编码后的字符串，所以可以直接写到文件中`f.write(str)`，查看文件的16进制内容为`D6D0B9FA`，这就是’中文‘二字的`gbk`码，如果我们想得到`str`的unicode字符串，需要对其解码`str.decode('gbk')`

python中字符串，包括unicode字符串，有两个编码（encode）/解码方法（decode），二者的作用分别是：

    str.encode(encoding='ascii') # 编码，将字符串按照encoding编码，一般unicode string使用
    str.decode(encoding='ascii') # 解码，将字符串按照encoding解码，一般非unicode string使用

示例代码如下：

    str = '中文' #基本字符串，带平台默认编码
    u_str = u'中文' #unicode字符串，’无编码‘，等价于 u'\u4e2d\u6587' 
    str.decode('gbk') #编程unicode字符串，等价于u_str
    u_str.encode('gbk') #使用gbk编码，等价于str
    str.decode('gbk').encode('utf-8') # 转化编码

### Java中的编码

`Java`中对字符串编码等设计的比较规范，所有的字符串内部存储使用`unicode`字符集及`UTF-16`的编码方式，因此，所有字符都是`16 bits`的

但16位的`UTF-16`最多只能编码`2^16=65536 `个字符`[0x0000-0xFFFF]`，但`unicode`字符集要大于65536，既然Java使用`UTF-16`，那么其如何表示剩下的`unicode`字符呢？

对于剩余的`unicode`字符，使用两个`java`字符表示，称为`surrogate representation`，如`U+20000 --> \uD840\uDC00`

假如字符串中含有这些特殊的`unicode`字符，使用`string.length()`会得到`Java`字符的个数，也就是说`U+20000`的`length()=2`

[reference of stackoverflow question](http://stackoverflow.com/questions/2533097/java-unicode-encoding/2639931#2639931)

### 编程中Charset和Encoding的区别

[问题](http://www.zhihu.com/question/21887246)

字符集（Charset）和编码方式（Encoding）区别很明显，顾名思义，字符集就是字符的集合，如ASCII，GBK，BIG5，Unicode等，编码方式是即可理解为定义在字符集上的映射规则

对于unicode字符集，有utf8,utf16,utf32等多种编码方式，由于unicode是其他字符集的超集，所以unicode和其他字符集的字符映射关系是确定的，可以方便的互相转换

然而，对于其他字符集，如 ASCII、GB2312、Big5 之类，属于不同国家在特定时期的字符编码解决方案（正在不断遗弃），这些字符集的编码方式基本是锁定的，比如ASCII码既是字符集，又是编码方案

Charset的发展：

    1. 最开始是ascii码（american standard code of information interchange 美国标准信息交换码），由7bits组成，再加上1个校验位，组成一个byte字节，共可以表示128个字符，当时已经够用了
    2. 当计算机国际化时，各国语言都要编码，这时候ascii不够用了，由此出现了 MBCS ( Multi-Byte Character System，多字节字符系统)，各国都建立自己的字符集，如中国的gb编码，日本的gis编码，中国台湾的big5编码等等
    3. 各国各自为政的编码体系严重阻碍国际化，由此有个叫 Unicode Consortium 的机构，决定做一个囊括所有字符在内的字符集(UCS, Universal Character Set)和对应编码方式的标准，即 Unicode

UCS字符集上的编码方式分为两种：等长编码和变长编码。由于UCS很大，等长编码使得单个字符编码较长，不适合存储和传输，而变长编码就很好的解决了这个问题（结合哈夫曼编码）。其中最为著名应用也最广的就是utf-8编码。UTF-8 (UCS Transformation Format — 8-bit[1]) is a variable-width encoding that can represent every character in the Unicode character set. 


