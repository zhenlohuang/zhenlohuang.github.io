---
title: Spark常见问题(持续更新)
tags: Spark
categories:
  - Big Data
  - Spark
date: 2019-04-21 20:41:15
---


# 综述
这篇文章记录了Spark应用开发过程遇到的各种问题及解决方案。主要来自于个人开发的实践，也有部分解决方案来自互联网，如有不足之处欢迎批评指正。**本人会持续更新转载请保留原文地址**

# 问题描述及解决方案
## java.lang.OutOfMemoryError: Java heap space错误
这个是Spark应用常见错误。JVM堆内存空间不足。解决方案如下：
* 首先要判断是Driver或者Executor出现OOM，通过--driver-memory或者--executor-memory进行调整。
* 如果是Spark SQL或者Spark Streaming的程序，建议适当地提高heap size。

## java.lang.OutOfMemoryError: GC overhead limit exceeded错误
这个也是Spark应用常见错误，由于GC时间过长导致的。解决方案如下：
* 直接通过--driver-memory或者--executor-memory增加heap size
* 修改GC policy。可以使用-XX:UseG1GC或者-XX:UseParallelGC

## 编译OK，运行时出NoClassDefineError错误
这个错误非常清晰，根本原因就是jar没有放入classpath之中。首先需要判断到底是Driver还是Executor缺少这个jar包。
* 将jar包路径配置到spark.driver.extraClassPath或者spark.executor.extraClassPath。
* 将jar包路径通过spark-submit的--driver-class-path或者--executor-class-path指定。

其实，spark-submit还有一个--packages参数，这个参数让Spark通过Maven从本地或者远程的repository处获取jar包。这个参数看似非常方便，但实际使用的时候不是很实用。因为Hadoop和Spark集群应用一般都是部署在内网的，为了数据安全，一般情况都是无法访问外网的。

## org.apache.spark.SparkException: Task not serializable
关于这个问题有单独的一篇文章进行分析，详见：[Spark Troubleshooting - Task not serializable问题分析](http://www.yidooo.net/2018/08/19/spark-task-not-serializable-analysis.html)

## java.io.IOException: No space left on device错误
具体stack trace如下：
```
stage 89.3 failed 4 times, most recent failure:
Lost task 38.4 in stage 89.3 (TID 30100, rhel4.cisco.com): java.io.IOException: No space left on device
            at java.io.FileOutputStream.writeBytes(Native Method)
            at java.io.FileOutputStream.write(FileOutputStream.java:326)
            at org.apache.spark.storage.TimeTrackingOutputStream.write(TimeTrackingOutputStream.java:58)
            at java.io.BufferedOutputStream.flushBuffer(BufferedOutputStream.java:82)
            at java.io.BufferedOutputStream.write(BufferedOutputStream.java:126)
```
这个错误是由于，Spark "scratch" space不足，具体路径通过spark.local.dir参数设置，默认是/tmp。官方对于scratch space的解释是
```
Directory to use for "scratch" space in Spark, including map output files and RDDs that get stored on disk.
This should be on a fast, local disk in your system.
It can also be a comma-separated list of multiple directories on different disks.
```

## spark.driver.maxResultSize超出错误
数据拉回Driver端是有限制的，通过spark.driver.maxResultSize控制：
* 默认是1g
* 可以设置为0或者unlimited
* 如果设置成unlimited就不会再遇到这个错误，取而代之的是OOM。

## java.lang.IllegalArgumentException: Size exceeds Integer.MAX_VALUE
具体stack trace如下：
```
java.lang.IllegalArgumentException: Size exceeds Integer.MAX_VALUE
at sun.nio.ch.FileChannelImpl.map(FileChannelImpl.java:828) at
org.apache.spark.storage.DiskStore.getBytes(DiskStore.scala:123) at
org.apache.spark.storage.DiskStore.getBytes(DiskStore.scala:132) at
org.apache.spark.storage.BlockManager.doGetLocal(BlockManager.scala:51 7) at
org.apache.spark.storage.BlockManager.getLocal(BlockManager.scala:432) at
org.apache.spark.storage.BlockManager.get(BlockManager.scala:618) at
org.apache.spark.CacheManager.putInBlockManager(CacheManager.scala:146 ) at
org.apache.spark.CacheManager.getOrCompute(CacheManager.scala:70)
```
这个问题是由于shuffle block大于2GB导致的，这个是Spark实现上的一个问题。Spark使用ByteBuffer作为storing blocks。
``` java
val buf = ByteBuffer.allocate(length.toInt) * ByteBufferislimitedbyInteger.MAX_SIZE
```
这就是2GB的由来。

# 参考资料
* http://spark.apache.org/docs/latest/configuration.html