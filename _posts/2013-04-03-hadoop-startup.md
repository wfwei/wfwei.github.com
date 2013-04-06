---
layout: post
title: "hadoop startup"
category: posts
---
##单节点安装
主要参考了[link](http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/)的文章，下面记录一下遇到的问题和具体安装情况
 
  * 最好新建一个专门hadoop用户，这样可以避免很多权限，隐私等问题，同时也很好将hadoop管理和运行分开，如新建一个hduser安装hadoop并负责启动和管理hadoop，其他用户只需将程序在hadoop上跑。
  * 貌似以前用`apt-get install`过hadoop？hadoop的配置到处都是，导致在安装之后无法运行，比如总是去/etc/hadoop找配置文件，之后找不到就用默认的，反正是不使用刚安装的（安装目录下的conf），仔细排查可能的配置，其实主要是几个配置文件用户目录下的`~/.bashrc ~/.bash_profile ~/.profile`更关键是全局的配置`/etc/profile`,之后在`/etc/profile`中找到了`HADOOP_CONF_DIR`的错误设置,删掉之后就好了
  * 学到一个找bug的方法，就是在启动程序中使用`echo $XXX`找bug，看看哪个变量不是预想的
  * 最后顺利安装，hadoop程序和配置都放在/usr/local/hadoop目录下
