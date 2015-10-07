---
layout: post
title: 'Hadoop 2.2.0安装及配置'
date: 2013-10-30
wordpress_id: 4357
permalink: /archives/hadoop-2-2-0-installation-and-configuration.html
categories: [Big Data]
tags: [Hadoop]
keywords: "Hadoop"
description: 
comments: true
---
# Pre-installation
保证所有主机上已经安装JDK 1.6+和ssh。

## 添加主机名到/etc/hosts
修改/etc/hosts

``` bash
sudo vi /etc/hosts
```
添加

``` bash
192.168.56.101 zhenlong-master
192.168.56.102 zhenlong-slave1
```

## 配置无密码的ssh连接
在所有主机上生成ssh的公钥和私钥

``` bash
ssh-keygen -t rsa
```
在master主机上，生成authorized_keys

``` bash
cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
chmod 644 ~/.ssh/authorized_keys
```
，并copy到所有slaves的~/.ssh/目录。

# Installation
## Download Apache Hadoop
*Apache Hadoop:* <http://www.apache.org/dyn/closer.cgi/hadoop/common/>

保证master和slaves上面的hadoop解压的相同的目录

## Set Environment Variables

``` bash
export HADOOP_HOME=/home/hadoop/hadoop-2.2.0
export JAVA_HOME=/home/hadoop/jdk1.6.0_45
```

## Hadoop Configuration
修改conf文件夹下面的几个文件：

- core-site.xml

``` xml
<configuration>
	<property>
		<name>fs.default.name</name>
		<value>hdfs://zhenlong-master:9000</value>
	</property>
	<property>
		<name>hadoop.tmp.dir</name>
		<value>/home/hadoop/hadoop_tmp/hadoop2</value>
	</property>
</configuration>
```

- hdfs-site.xml

``` xml
<configuration>
	<property>
		<name>dfs.namenode.name.dir</name>
		<value>/home/hadoop/hadoop_dfs/hadoop2/name</value>
		<description>Path on the local filesystem where the NameNode stores the namespace and transactions logs persistently.</description>
	</property>
	<property>
		<name>dfs.datanode.data.dir</name>
		<value>/home/hadoop/hadoop_dfs/hadoop2/data</value>
		<description>Comma separated list of paths on the local filesystem of a DataNode where it should store its blocks.</description>
	</property>
	<property>
		<name>dfs.permissions</name>
		<value>false</value>
	</property>
</configuration>
```
- mapred-site.xml

``` xml
<configuration>
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
</configuration>
```

- yarn-site.xml

``` xml
<configuration>
	<property>
		<name>yarn.resourcemanager.resource-tracker.address</name>
		<value>zhenlong-master:8031</value>
		<description>host is the hostname of the resource manager and    port is the port on which the NodeManagers contact the Resource Manager.          </description>
	</property>
	<property>
		<name>yarn.resourcemanager.scheduler.address</name>
		<value>zhenlong-master:8030</value>
		<description>host is the hostname of the resourcemanager and port is the port    on which the Applications in the cluster talk to the Resource Manager.          </description>
	</property>
	<property>
		<name>yarn.resourcemanager.scheduler.class</name>
		<value>org.apache.hadoop.yarn.server.resourcemanager.scheduler.capacity.CapacityScheduler</value>
		<description>In case you do not want to use the default scheduler</description>
	</property>
	<property>
		<name>yarn.resourcemanager.address</name>
		<value>zhenlong-master:8032</value>
		<description>the host is the hostname of the ResourceManager and the port is the port on    which the clients can talk to the Resource Manager. </description>
	</property>
	<property>
		<name>yarn.nodemanager.local-dirs</name>
		<value>${hadoop.tmp.dir}/nodemanager/local</value>
		<description>the local directories used by the nodemanager</description>
	</property>
	<property>
		<name>yarn.nodemanager.address</name>
		<value>0.0.0.0:8034</value>
		<description>the nodemanagers bind to this port</description>
	</property>
	<property>
		<name>yarn.nodemanager.remote-app-log-dir</name>
		<value>${hadoop.tmp.dir}/nodemanager/remote</value>
		<description>directory on hdfs where the application logs are moved to </description>
	</property>
	<property>
		<name>yarn.nodemanager.log-dirs</name>
		<value>${hadoop.tmp.dir}/nodemanager/logs</value>
		<description>the directories used by Nodemanagers as log directories</description>
	</property>
	<!-- Use mapreduce_shuffle instead of mapreduce.suffle (YARN-1229)-->
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
	<property>
		<name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
		<value>org.apache.hadoop.mapred.ShuffleHandler</value>
	</property>
</configuration>
```

- slaves

```
zhenlong-slave1
```

- hadoop-env.sh

``` bash
export JAVA_HOME=/home/hadoop/jdk1.6.0_45
```
此处JAVA_HOME可以根据每天Server情况设定。

## 格式化NameNode
第一次启动，需要先格式化NameNode。

``` bash
hadoop namenode -format
```

## Start Hadoop

``` bash
~/hadoop-2.2.0/sbin/start-all.sh
```
![image](/images/uploads/2013/10/start.png)

- In master

![image](/images/uploads/2013/10/master.png)

- In slaves

![image](/images/uploads/2013/10/slaves.png)

#  Test
- HDFS Web UI: http://zhenlong-master:50070

- YARN Web UI: http://zhenlong-master:8088
	
- YARN && MapReduce 测试：

``` bash
~/hadoop-2.2.0/bin/hadoop jar ~/hadoop-2.2.0/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.2.0.jar wordcount file wordcount_out
```

![image](/images/uploads/2013/10/yarn.png)