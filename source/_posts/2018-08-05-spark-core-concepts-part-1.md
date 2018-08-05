---
title: Spark核心概念 - Job、Stage、Task和Partition
tags: Spark
categories:
  - Big Data
  - Spark
date: 2018-08-05 16:02:08
---


# 概述
在我们编写Spark Application的的过程中，其实并不会涉及到Job、Stage等概念，因为Spark其进行了封装，使之对用户是透明的。然而，当用户Tuning Spark Application时，这些概念又是非常重要的概念。

# Job
所谓一个Job，就是由一个RDD的action触发的动作，可以简单的理解为，当你需要执行一个RDD的action的时候，会生成一个job。

# Stage
Stage是一个Job的组成单位，就是说，一个Job会被切分成1个或多个的Stage，然后各个Stage会按照执行顺序依次执行。至于Job根据什么标准来切分Stage，可以简单理解为按照是否需要shuffle来划分stage。当一个操作需要shuffle时，Spark就会将其划分为一个Stage。具体细节后续会有文章单独介绍。

# Task
Task是Spark的执行单元，每个Stage都会包含一个或者一组Task。一般来说，数据有多少个partition就会有多少个Task。

# Partition
Partition类似Hadoop的Split，是数据的一种划分方式，计算以Partition为单位进行的。Partition的划分依据有很多，也可以自己定义。例如HDFS的文件的划分方式就是按照HDFS Block大小来划分的。
