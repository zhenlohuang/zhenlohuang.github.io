---
layout: post
title: 'Hadoop源代码编译'
date: 2013-11-24
categories: [Big Data, Hadoop]
tags: [Hadoop]
keywords: "Hadoop"
description: 
comments: true
---
# Environment
OS: Ubuntu 12.04 LTS

Kernel Version： 3.8.0-33-generic

Hadoop： 2.2.0

# Pre-Compile
官方的BUILDING.txt给出的安装需求

![image](/images/legacy/2013/11/Selection_001.png)

## 安装JDK

``` bash
sudo apt-get install openjdk-7-jdk
```
## 安装Maven

``` bash 
sudo apt-get install maven
```
## 安装Findbugs
Download: <http://findbugs.sourceforge.net/>

设置环境变量

``` bash 
vi .bashrc
#set findbugs path
export FINDBUGS_HOME=/home/killua/Dev/findbugs-2.0.3
export PATH=$PATH:$FINDBUGS_HOME/bin
```
## 安装ProtocolBuffer
Download: <https://code.google.com/p/protobuf/>

在安装protocolbuffer之前需要先安装g++

``` bash 
sudo apt-get install g++
```
安装protocolbuffer

``` bash 
cd  protobuf-2.5.0
./configure
make
make install
```
## 安装CMake

``` bash 
sudo apt-get install cmake
```

# Compile

``` bash 
mvn package -Pdist -DskipTests -Dtar
```
![image](/images/legacy/2013/11/Screenshot-from-2013-11-24-171453.png)

编译后的结果可以在hadoop-2.2.0-src/hadoop-dist/target/hadoop-2.2.0中看到

![image](/images/legacy/2013/11/Selection_003.png)

# 常见问题
1） error: cannot access AbstractLifeCycle

在编译的过程中出现/home/killua/Workspace/hadoop-2.2.0-src/hadoop-common-project/hadoop-auth/src/test/java/org/apache/hadoop/security/authentication/client/AuthenticatorTestCase.java:[88,11] error: cannot access AbstractLifeCycle错误

经过查证发现是hadoop2.2.0的一个bug，具体参见<a title="https://issues.apache.org/jira/browse/HADOOP-10110 " href="https://issues.apache.org/jira/browse/HADOOP-10110 ">https://issues.apache.org/jira/browse/HADOOP-10110 </a>

解决方法：

修改hadoop-2.2.0-src/hadoop-common-project/hadoop-auth/pom.xml,将

``` xml
    <dependency>
       <groupId>org.mortbay.jetty</groupId>
       <artifactId>jetty</artifactId>
       <scope>test</scope>
    </dependency>
```
修为

``` xml
    <dependency>
       <groupId>org.mortbay.jetty</groupId>
       <artifactId>jetty-util</artifactId>
       <scope>test</scope>
    </dependency>
    <dependency>
       <groupId>org.mortbay.jetty</groupId>
       <artifactId>jetty</artifactId>
       <scope>test</scope>
    </dependency>
```
