---
layout: post
title: 'Apache Hive安装及配置'
date: 2013-11-6
wordpress_id: 4601
permalink: /archives/apache-hive-installation.html
categories: [Big Data]
tags: [Hive]
keywords: "Hive"
description: 
comments: true
---
# 安装前
在安装Hive之前，请保证已经安装了Hadoop。Hadoop安装参考：[Hadoop 2.2.0安装及配置](http://www.yidooo.net/archives/hadoop-2-2-0-installation-and-configuration.html)

# 安装Mysql
本文选用mysql作为Hive的metastore。

``` bash 
sudo yum install mysql-server
```
## 创建数据库

``` bash 
mysql> create database hive;
Query OK, 1 row affected (0.00 sec)
```
##修改数据库操作权限

``` bash 
mysql> grant all on hive.* to hive@'%'  identified by 'hive';
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

# Hive安装

``` bash 
tar zcvf hive-0.12.0.tar.gz hive-0.12.0
```

# Hive配置

``` bash 
cd conf
cp hive-default.xml.template hive-site.xml
cp hive-env.sh.template hive-env.sh
cp hive-log4j.properties.template hive-log4j.properties
cp hive-exec-log4j.properties.template hive-exec-log4j.properties
```
- hive-site.xml

``` xml
<property>
  <name>javax.jdo.option.ConnectionURL</name>
  <value>jdbc:mysql://localhost:3306/hive</value>
  <description>JDBC connect string for a JDBC metastore</description>
</property>

<property>
  <name>javax.jdo.option.ConnectionDriverName</name>
  <value>com.mysql.jdbc.Driver</value>
  <description>Driver class name for a JDBC metastore</description>
</property>

<property>
  <name>javax.jdo.option.ConnectionUserName</name>
  <value>root</value>
  <description>username to use against metastore database</description>
</property>

<property>
  <name>javax.jdo.option.ConnectionPassword</name>
  <value>welcome123</value>
  <description>password to use against metastore database</description>
</property>

<property>
  <name>hive.metastore.schema.verification</name>
  <value>false</value>
   <description>
   Enforce metastore schema version consistency.
   True: Verify that version information stored in metastore matches with one from Hive jars.  Also disable automatic
         schema migration attempt. Users are required to manully migrate schema after Hive upgrade which ensures
         proper metastore schema migration. (Default)
   False: Warn if the version information stored in metastore doesn't match with one from in Hive jars.
   </description>
</property>
```
- hive-env.sh

``` bash 
# Set HADOOP_HOME to point to a specific hadoop install directory
HADOOP_HOME=/home/hadoop/hadoop-2.2.0

# Hive Configuration Directory can be controlled by:
export HIVE_CONF_DIR=/home/hadoop/hive-0.12.0/conf
```

# 安装Mysql JDBC Connector
下载页面：<http://www.mysql.com/downloads/connector/j/5.1.html>

``` bash 
cp mysql-connector-java-5.1.26-bin.jar to hive/lib
```

# 测试

``` bash 
hive> create table test (key string);
OK
Time taken: 1.09 seconds
```

``` bash 
hive> create table test (key string);
hive> show tables;
OK
test
Time taken: 0.084 seconds, Fetched: 1 row(s)
```

# 常见错误
**错误：ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)**    
**解决方法：**sudo service mysqld start

**错误：ERROR 1044 (42000): Access denied for user ''@'localhost' to database 'hive'**    
**解决方法:**    
[hadoop@zhenlong-master ~]$ mysql -h localhost -u root -p
Enter password:

**错误：FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. java.lang.RuntimeException: Unable to instantiate org.apache.hadoop.hive.metastore.HiveMetaStoreClient**
这个错误的原因很多，因此需要进行调试。 启动hive带上调试参数，./hive -hiveconf hive.root.logger=DEBUG,console，从调试信息中可以获得错误详细信息。
如果错误信息为：
Caused by: org.datanucleus.exceptions.NucleusException: Attempt to invoke the "BoneCP" plugin to create a ConnectionPool gave an error : The specified datastore driver ("com.mysql.jdbc.Driver") was not found in the CLASSPATH. Please check your CLASSPATH specification, and the name of the driver.   
*解决方法：*将mysql的jdbc driver拷贝到hive/lib即可。   
如果错误信息为：   
Caused by: MetaException(message:Version information not found in metastore. )   
*解决方法：*set hive.metastore.schema.verification = false   

``` xml
<property>
   <name>hive.metastore.schema.verification</name>
   <value>false</value>
    <description>
    Enforce metastore schema version consistency.
    True: Verify that version information stored in metastore matches with one from Hive jars.  Also disable automatic
          schema migration attempt. Users are required to manully migrate schema after Hive upgrade which ensures
          proper metastore schema migration. (Default)
    False: Warn if the version information stored in metastore doesn't match with one from in Hive jars.
    </description>
 </property>
```


