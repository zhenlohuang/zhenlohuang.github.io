---
title: Spark架构简介
tags: Spark
categories:
  - Big Data
  - Spark
date: 2018-07-29
---

# Spark 架构简介
Spark是一个master/slave架构的分布式系统，它的架构主要包含有
* Spark Driver
* Spark Executor
* Cluster Manager
![spark_architecture.png](/images/spark_architecture.png)

一个Spark集群一般拥有单个的Driver和多个的Executor。Spark Driver和Executor都是独立运行的JVM进程，它们可以运行在单台机器上，也可以运行在多台机器上。

# Spark Driver
Spark Driver是一个Spark Application的主入口，它可以用Scala，Python或者R进行编写。一个Spark Driver包含有一个SparkContext，这是整个Spark Application中最核心的组件。同时，还包含有DAGScheduler, TaskScheduler, BackendScheduler和BlockManager等组件用于将用户代码转换为Spark job运行在集群当中。Spark Driver的主要功能包括：
* 负责协调Job的运行和以及Cluster Manager进行交互。
* 将RDD转换为执行的DAG图，同时把DAG图分为不同的Stage
* 将Job切割成更小的执行单元，Task，由Executor执行。
* 启动一个HTTP Server，端口为4040。这个Web UI会把Spark Application运行时的信息展示出来。

# Spark Executor
Spark Executor是Task的实际执行者。每个Application的Executor数量可以通过配置指定（Static Allocation）或者有Spark动态分配（Dynamic Allocation）。Executor的主要功能包括：
* 负责所有的数据处理工作
* 用于读取和写入外部数据源
* 缓存着计算过程中的数据

# Cluster Manager
严格上说Cluster Manager并不是Spark的一部分，而是一个外部的Service（除了Standalone）。Spark Driver会和其进行交互用于从集群里获取资源（CPU，Memory等）。目前Spark支持4种Cluster Manager：
* Standalone：这是一种Spark自带的集群管理模式，设计也比较简单。
* Apache Mesos：Mesos是一种通用的集群资源管理服务，用于管理MapReduce应用或者其他类型的应用。
* Hadoop YARN ：YARN是由Hadoop 2.0引入的集群资源管理服务。
* Kubernetes：Kubernetes是一种管理containerized的应用的服务。Spark 2.3以后引入了对Kubernetes的支持。

至于选择使用哪一种Cluster Manager，完全取决于生产环境以及业务场景，并没有绝对的优劣。