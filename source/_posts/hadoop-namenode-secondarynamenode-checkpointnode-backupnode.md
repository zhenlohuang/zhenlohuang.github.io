---
layout: post
title: '浅谈Hadoop NameNode、SecondaryNameNode、CheckPoint Node和BackupNode'
date: 2013-11-16
wordpress_id: 4815
categories: [Big Data, Hadoop]
tags: [Hadoop, HDFS]
keywords: "Hadoop, HDFS"
description: 
comments: true
---
# NameNode
Hadoop NameNode管理着文件系统的Namespace，它维护着整个文件系统树（FileSystem Tree）以及文件树中所有的文件和文件夹元数据（Metadata）。

Namenode Metadata主要是两个文件：edits和fsimage。fsimage是HDFS的最新状态（截止到fsimage文件创建时间的最新状态）文件，而edits是自fsimage创建后的namespace操作日志。Namenode每次启动的时候，都要合并两个文件，按照edits的记录，把fsimage文件更新到最新。

# Secondary NameNode
Hadoop SecondaryNameNode并不是Hadoop 第二个NameNode，它不提供NameNode服务，而仅仅是NameNode的一个工具，帮助NameNode管理Metadata数据。

一般情况下，当NameNode重启的时候，会合并硬盘上的fsimage文件和edits文件，得到完整的Metadata信息。但是，如果集群规模十分庞大，操作频繁，那么edits文件就会非常大，这个合并过程就会非常慢，导致HDFS长时间无法启动。如果定时将edits文件合并到fsimage，那么重启NameNode就可以非常快，而SecondaryNameNode就做这个合并的工作。

SecondaryNamenode定期地从Namenode上获取元数据。当它准备获取元数据的时候，就通知Namenode暂停写入edits文件。Namenode收到请求后停止写入edits文件，之后的log记录写入一个名为edits.new的文件。Secondary Namenode获取到元数据以后，把edits文件和fsimage文件在本机进行合并，创建出一个新的fsimage文件，然后把新的fsimage文件发送回Namenode。Namenode收到Secondary Namenode发回的fsimage后，就拿它覆盖掉原来的fsimage文件，并删除edits文件，把edits.new重命名为edits。

通过这样一番操作，就避免了Namenode的edits日志的无限增长，加速Namenode的启动过程。
![image](/images/uploads/2013/11/secondarynamenode.png)

# CheckPoint Node
可能是由于Secondary NameNode容易对人产生误导，因此Hadoop 1.0.4 之后建议不要使用Secondary NameNode，而使用CheckPoint Node。Checkpoint Node和Secondary NameNode的作用以及配置完全相同，只是启动命令不同 bin/hdfs namenode -checkpoint

# Backup Node

Backup Node在内存中维护了一份从Namenode同步过来的fsimage，同时它还从namenode接收edits文件的日志流，并把它们持久化硬盘，Backup Node把收到的这些edits文件和内存中的fsimage文件进行合并，创建一份元数据备份。虽然BackupNode是一个备份的NameNode节点，不过Backup Node目前还无法直接接替NameNode提供服务。因此当前版本的Backup Node还不具有热备功能，也就是说，当NameNode发生故障，目前还只能通过重启NameNode的方式来恢复服务。

不过在Hadoop 2.x中提出了Hadoop HA的一些策略，实现了Hadoop NameNode的failover。


