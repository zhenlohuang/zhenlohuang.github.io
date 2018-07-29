---
layout: post
title: 'Hadoop安装及部署'
date: 2012-10-29
categories: [Big Data, Hadoop]
tags: [Hadoop]
keywords: "Hadoop"
description: 
comments: true
---
OS: Ubuntu 12.04  
Hadoop: Hadoop 1.0.4

**1.安装集群所需软件**

```
sudo apt-get install install ssh
sudo apt-get install rsync
```
配置ssh免密码登录

```
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
cat ~/.ssh/id_dsa.pub >>~/.ssh/authorized_keys
```
验证是否成功

```
ssh localhost
```

**2.安装JDK**

安装部分就不重复了，主要说明下环境变量的添加

```
sudo vi /etc/profile
```
将下面三句话添加到最后

```
export JAVA_HOME=/usr/lib/jvm/java-6-openjdk
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

**3.安装Hadoop**

下载地址：<http://apache.etoak.com/hadoop/common/> 建议选择：1.0.x版本的tar文件，不建议使用deb或者rpm的包，因为后面回带来很复杂的程序权限问题。
修改配置文件，指定JDk安装路径

```
vi conf/hadoop-env.sh
```

```
export JAVA_HOME=/usr/lib/jvm/java-6-openjdk
```
修改Hadoop核心配置文件core-site.xml，这里配置的是HDFS的地址和端口号

```
vi conf/core-site.xml
```

``` xml
<pre class="brush: xml; gutter: true; first-line: 1"><configuration>
     <property>
         <name>fs.default.name</name>
         <value>hdfs://localhost:9000</value>
     </property>
<property>
    <name>hadoop.tmp.dir</name>
    <value>/home/killua/Application/tmp/hadoop-${user.name}</value>
  </property>
</configuration>
```
修改Hadoop中HDFS的配置，配置的备份方式默认为3，因为安装的是单机版，所以需要改为1
vi conf/hdfs-site.xml

``` xml
<configuration>
     <property>
         <name>dfs.replication</name>
         <value>1</value>
     </property>
</configuration>
```
修改Hadoop中MapReduce的配置文件，配置的是JobTracker的地址和端口

```
vi conf/mapred-site.xml
```

``` xml
<configuration>
	<property>
		<name>mapred.job.tracker</name>
		<value>localhost:9001</value>
	</property>
  </configuration>
```
启动Hadoop，在启动之前，需要格式化Hadoop的文件系统HDFS

```
bin/hadoop namenode -format
然后启动Hadoop所有服务，输入命令
```
bin/start-all.sh

```
**4.验证是否安装成功**
打开浏览器，分别输入一下网址：
http://localhost:50030 (MapReduce的Web页面)
http://localhost:50070 (HDfS的web页面)
```
