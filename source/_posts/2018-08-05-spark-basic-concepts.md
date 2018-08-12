---
title: Spark基本概念
tags: Spark
categories:
  - Big Data
  - Spark
date: 2018-08-05 16:02:08
---


# 概述
在我们编写Spark Application的的过程中，会涉及到很多概念，下面会介绍一些基本概念用于更好地理解Spark的设计以及应用开发。

# RDD（Resillent Distributed Dataset）
弹性数据集。它是Spark的基本数据结构，Spark中的所有数据都是通过RDD的形式进行组织。RDD是不可变的数据集合，不可变的意思是RDD中的每个分区数据是只读的。RDD数据集是要做逻辑分区的，每个分区可以单独在集群节点进行计算。

# DAG（Directed Acycle Graph）
有向无环图。在图论中，如果一个有向图无法从有一个定点出发经过若干条边回到该点，则这个图是有向无环图。Spark中用DAG来表示RDD之间的血缘关系。

# NarrowDependency
窄依赖，子RDD依赖于父RDD中固定的Partition。NarrowDependency分为OneToOneDependency和RangeDependency。

# ShuffleDependency
Shuffle依赖，也称作宽依赖，子RDD可能对父RDD中所有的Partition都有依赖。子RDD对父RDD各个Partition的依赖取决于分区计算器（Partitioner）的算法。

# Job
所谓一个Job，就是由一个RDD的action触发的动作，可以简单的理解为，当你需要执行一个RDD的action的时候，会生成一个job。

# Stage
Stage是一个Job的组成单位，就是说，一个Job会被切分成1个或多个的Stage，然后各个Stage会按照执行顺序依次执行。至于Job根据什么标准来切分Stage，可以简单理解为按照是否需要shuffle来划分stage。当一个操作需要shuffle时，Spark就会将其划分为一个Stage。具体细节后续会有文章单独介绍。

# Task
Task是Spark的执行单元，每个Stage都会包含一个或者一组Task。一般来说，数据有多少个partition就会有多少个Task。

# Partition
Partition类似Hadoop的Split，是数据的一种划分方式，计算以Partition为单位进行的。Partition的划分依据有很多，也可以自己定义。例如HDFS的文件的划分方式就是按照HDFS Block大小来划分的。

# Shuffle
Shuffle是Spark Application中一个非常重要的阶段。Shuffle用于打通map task（在Spark称为ShuffleMapTask）的输出与reduce task的任务（在Spark中就是ResultTask）的输入，map task的输出结果按照指定的分区策略分配给某一个分区的reduce task。
