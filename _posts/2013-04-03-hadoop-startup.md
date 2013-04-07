---
layout: post
title: "hadoop startup"
category: posts
---

## java.lang.OutOfMemoryError: GC overhead limit exceeded
  * 在hadoop上跑canopy聚类的时候，发生错误`java.lang.OutOfMemoryError: GC overhead limit exceeded`,虽然继续运行，但是后来又抛出`heap size overflow`，可见是程序占用堆空间太多，又释放不掉的缘故
  * 这个错误主要是说，GC一直在忙着回收堆上的垃圾，占用大量cpu(>98%)，但是回收效率极低(<2%)
  * 也就是说程序已经没有什么进展，一直在忙着回收垃圾，为了避免这种浪费，jvm抛出错误

## Hadoop 权限问题
  * 刚搭建好hadoop，发现除hduser之外的账户不能跑hadoop，权限问题
  * 需要用hduser账户把其他要运行hadoop程序的账户添加到hdfs上，具体方法是在hdfs上新建用户工作目录
    
    hadoop dfsadmin -safemode leave //关闭安全模式
    hadoop fs -mkdir /user/uname // 创建用户工作目录
    hadoop fs -chown -R uname:group /user/uname //修改权限

## Connection refused 
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
