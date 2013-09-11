---
layout: post
title: "Hadoop"
categories: [tool, hadoop]
---

Hadoop 权威指南 笔记

### 初识Hadoop

#### 大量数据产生,磁盘容量增长快速的同时，访问速度却未能与时俱进

>1TB的数据，传输速度100M/s，大概需要2个小时

一个简单思路是将数据放到多个不同磁盘上，同时读取，可以缩短读取时间

>使用100个磁盘，每个磁盘存放1%的数据，同时读取的话，只需要2分钟

这样又出现新的问题：

1. 如何解决高频硬件故障：硬件增加后，系统发生故障的概率就大大增加，需要做好系统恢复机制，hadoop的分布式文件系统能很好的handle这个
2. 如何有效利用多个磁盘上的数据：MapReduce这种分布式编程框架，允许结合多个数据源的数据进行运算，且保证正确性

####寻址时间的提高远远慢于传输速度的提高

> 寻址时间是将磁头移动到特定磁道的过程，是磁盘操作延迟的主要原因

Hadoop使用大量的流式文件读取，其读取速度取决与磁盘速度

传统的关系型数据库使用B树做数据存储，在少量数据更新时，B树的效率较高，但数据库系统更新大部分数据时，B树的效率就远不及MapReduce了，因为其还需要‘排序/归并’来重建数据库

这也是为什么hdfs的块非常大（一般64M）的原因

####MapReduce vs MPI

消息传递接口(Message Passing Interface, MPI)

### 关于 MapReduce

MapReduce是一种用于数据处理的编程模型，其优势在于处理大规模数据集

MapReduce任务被分成两个阶段：map阶段和reduce阶段。每个阶段都已键值对作为输入输出，并可以选择其类型，程序员只需要编写对应的map和reduce函数

### 关于Hadoop
* Hadoop 是一个实现了MapReduce 计算模型的开源分布式并行编程框架
* Hadoop = HDFS+ MapReduce
* Hadoop HDFS
  * Google GFS存储系统的开源实现
  * 作为并行计算环境（MapReduce）的基础组件，同时也是BigTable（如HBase、HyperTable）的底层分布式文件系统。
  * master/slave架构，HDFS集群是有由一个Namenode和一定数目的Datanode组成。
  * Namenode是一个中心服务器，负责管理文件系统的namespace和客户端对文件的访问。
  * Datanode在集群中一般是一个节点一个，负责管理节点上它们附带的存储。
* Hadoop MapReduce
  * 一个MapReduce作业（job）通常会把输入的数据集切分为若干独立的数据块
  * Map任务（task）以完全并行的方式处理它们
  * 框架会对Map的输出先进行排序，然后把结果输入给Reduce任务
  * 通常作业的输入和输出都会被存储在文件系统中
* Hive
  * 基于Hadoop的一个数据仓库工具，处理能力强而且成本低廉 
  * 将结构化的数据文件映射为一张数据库表
  * 提供类SQL语言，实现完整的SQL查询功能
  * 可以将SQL语句转换为MapReduce任务运行，十分适合数据仓库的统计分析
  * 采用__行__存储的方式（SequenceFile）来存储和读取数据，效率低
* HBase
  * 分布式、面向__列__的开源数据库
  * 适合于非结构化数据存储

### java.lang.OutOfMemoryError: GC overhead limit exceeded
  * 在hadoop上跑canopy聚类的时候，发生错误`java.lang.OutOfMemoryError: GC overhead limit exceeded`,虽然继续运行，但是后来又抛出`heap size overflow`，可见是程序占用堆空间太多，又释放不掉的缘故
  * 这个错误主要是说，GC一直在忙着回收堆上的垃圾，占用大量cpu(>98%)，但是回收效率极低(<2%)
  * 也就是说程序已经没有什么进展，一直在忙着回收垃圾，为了避免这种浪费，jvm抛出错误

### Hadoop 权限问题
  * 刚搭建好hadoop，发现除hduser之外的账户不能跑hadoop，权限问题
  * 需要用hduser账户把其他要运行hadoop程序的账户添加到hdfs上，具体方法是在hdfs上新建用户工作目录
    
    hadoop dfsadmin -safemode leave //关闭安全模式
    hadoop fs -mkdir /user/uname // 创建用户工作目录
    hadoop fs -chown -R uname:group /user/uname //修改权限

### Connection refused 
* hadoop突然不能跑了，报错大致如下:
`Call to localhost/127.0.0.1:54310 failed on connection exception: java.net.ConnectException: Connection refused`
使用`jps`一看，namenode没有启动，原来是自己删了hadoop.tmp.dir,之后没有重新格式化namenode，导致namenode启动不了，重新格式化，`hadoop namenode -format`后启动`start-all.sh`，成功~

* 还有另外一个错误会导致这种错，貌似是hadoop和ipv6绑定的问题，[参考](http://stackoverflow.com/questions/8501781/errors-while-running-hadoop), 解决方法是添加`export HADOOP_OPTS=-Djava.net.preferIPv4Stack=true`到`$HADOOP_HOME/conf/hadoop-env.sh`


##单节点安装
主要参考了[link](http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/)的文章，下面记录一下遇到的问题和具体安装情况
 
  * 最好新建一个专门hadoop用户，这样可以避免很多权限，隐私等问题，同时也很好将hadoop管理和运行分开，如新建一个hduser安装hadoop并负责启动和管理hadoop，其他用户只需将程序在hadoop上跑。
  * 貌似以前用`apt-get install`过hadoop？hadoop的配置到处都是，导致在安装之后无法运行，比如总是去/etc/hadoop找配置文件，之后找不到就用默认的，反正是不使用刚安装的（安装目录下的conf），仔细排查可能的配置，其实主要是几个配置文件用户目录下的`~/.bashrc ~/.bash_profile ~/.profile`更关键是全局的配置`/etc/profile`,之后在`/etc/profile`中找到了`HADOOP_CONF_DIR`的错误设置,删掉之后就好了
  * 学到一个找bug的方法，就是在启动程序中使用`echo $XXX`找bug，看看哪个变量不是预想的
  * 最后顺利安装，hadoop程序和配置都放在/usr/local/hadoop目录下
