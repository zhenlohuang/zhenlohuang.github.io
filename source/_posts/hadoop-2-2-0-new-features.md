---
layout: post
title: 'Hadoop 2.2.0新特性简介'
date: 2013-10-30
categories: [Big Data, Hadoop]
tags: [Hadoop]
keywords: "Hadoop"
description: 
comments: true
---
2013年10月15日，Apache Hadoop发布了2.2.0版本。这个是里程碑式的版本，它是Hadoop 2.0的第一个stable版本，它标志着Hadoop从此进入了2.0时代。

- YARN

YARN（Yet Another Resource Negotiator），也称MapReduce 2.0，它是Hadoop 2.0引入的一个全新的通用资源管理系统，可以在其上运行各种程序和框架。YARN是从MapReduce 1.0衍化来的。引入了ResourceManager、ApplicationMaster和NodeManager等组件用于系统的资源调度。

- HDFS High Availability

Hadoop HA同时解决了NameNode单点故障问题和内存受限问题。主要利用Active NameNode和Standby NameNode之间的状态切换解决单节点故障问题。而其中采用NFS、QJM和Bookeeper三种可选策略用于NameNode之间的Metadata共享。

- HDFS Federation

HDFS Federation，它允许一个HDFS集群中存在多个NameNode，每个NameNode分管一部分目录，而不同NameNode之间彼此独立，共享所有DataNode的存储资源，由于NameNode Federation中的每个NameNode仍存在单点问题，需为每个NameNode提供一个backup以解决单点故障问题。

- HDFS Snapshots

HDFS Snapshots，是HDFS在某一时刻的只读镜像。它可以是整个文件系统也可以是一部分文件夹。通常用于数据备份，防止数据丢失或者误操作。

- NFSv3

将NFS引入HDFS后，用户可像读写本地文件一样读写HDFS上的文件，大大简化了HDFS使用。

- 支持Windows平台

在2.2.0版本之前，Hadoop仅支持Linux操作系统，而Windows仅作为实验平台使用。将Microsoft有关Hadoop Windows Patch吸收进代码之后，Hadoop对Windows的支持有了本质上的增强。其实之前Hortonworks的HDP的也曾发布支持Windows的Hadoop。

- 兼容Hadoop 1.x的MapReduce程序

为了使MRv1可以在YARN架构上运行，对MR API进行了修改保证可以在YARN上运行。

- 完成与Hadoop生态圈的其他软件的集成测试

相比Hadoop 1.0中，Hadoop 2.0对HDFS和MapReduce等核心组件进行了较大的改动，并引入了全新的资源调度框架YARN。因此对那些以Hadoop作为底层的应用进行集成测试是十分必要的。

参考资料：

Apache Hadoop 2.2.0: <http://hadoop.apache.org/docs/r2.2.0/>

Hadoop 2.0稳定版本2.2.0新特性剖析: <http://dongxicheng.org/mapreduce-nextgen/hadoop-2-2-0/>

HDFS Snapshots: <http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsSnapshots.html>

Hadoop For Windows: <http://dongxicheng.org/mapreduce/hadoop-for-windows/>