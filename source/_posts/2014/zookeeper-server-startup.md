---
layout: post
title: "Zookeeper源码分析-Zookeeper Server启动分析"
date: 2014-09-22 22:52:55 +0800
categories: [Distributed System]
tags: [ZooKeeper]
comments: true
---

Zookeeper Server的启动入口为org.apache.zookeeper.server.quorum.QuorumPeerMain。Zookeeper的启动模式分为两种：一种为standalone mode；另一种为cluster mode。

- Standalone模式：当配置文件中仅配置了一台server时，Zookeeper将以standalone模式启动，启动类为ZooKeeperServerMain，此处不作详细分析。
- Cluster模式：当配置文件中配置了多台server，构建cluster，启动类为QuorumPeer#start()。

``` java
public synchronized void start() {
        loadDataBase();
        cnxnFactory.start();        
        startLeaderElection();
        super.start();
    }
``` 

# 清理DataDir
在Server启动时，根据启动的配置参数启动一个TimeTask用于清理DataDir中的snapshot及对应的transactionlog。由于Zookeeper的任何一个写操作都将在transaction log中留下记录，当写操作达到一定量或者一定时间间隔，Zookeeper将transaction log合并为snapshot。所以随着运行时间的增长生成的transaction log和snapshot将越来越多，所以定期清理是必要的。在DatadirCleanupManager中有两个参数：

- snapRetainCount：清理后保留的snapshot的个数，该参数至少要大于3。
- purgeInterval：定期清理的时间间隔，以小时为单位。

``` java
	// Start and schedule the the purge task
        DatadirCleanupManager purgeMgr = new DatadirCleanupManager(config
                .getDataDir(), config.getDataLogDir(), config
                .getSnapRetainCount(), config.getPurgeInterval());
        purgeMgr.start();
```

# 加载ZKDatabase
1.从snapshot和transaction log中恢复ZKDatabase，并将其载入内存。

``` java
zkDb.loadDataBase();
```
2.载入currentEpoch和acceptedEpoch

首先要明白什么是epoch，官方给出的解释是:
>The zxid has two parts: the epoch and a counter. In our implementation the zxid is a 64-bit number. We use the high order 32-bits for the epoch and the low order 32-bits for the counter. Because it has two parts represent the zxid both as a number and as a pair of integers, (epoch, count). The epoch number represents a change in leadership. Each time a new leader comes into power it will have its own epoch number. 

从currentEpoch文件中读取current epoch；若currentEpoch文件不存在，Zookeeper将从lastProcessedZxid中获取epoch作为current epoch，写入currentEpoch文件。

``` java
	try {
            	currentEpoch = readLongFromFile(CURRENT_EPOCH_FILENAME);
                if (epochOfZxid > currentEpoch && updating.exists()) {
                    LOG.info("{} found. The server was terminated after " +
                             "taking a snapshot but before updating current " +
                             "epoch. Setting current epoch to {}.",
                             UPDATING_EPOCH_FILENAME, epochOfZxid);
                    setCurrentEpoch(epochOfZxid);
                    if (!updating.delete()) {
                        throw new IOException("Failed to delete " +
                                              updating.toString());
                    }
                }
            } catch(FileNotFoundException e) {
            	// pick a reasonable epoch number
            	// this should only happen once when moving to a
            	// new code version
            	currentEpoch = epochOfZxid;
            	LOG.info(CURRENT_EPOCH_FILENAME
            	        + " not found! Creating with a reasonable default of {}. This should only happen when you are upgrading your installation",
            	        currentEpoch);
            	writeLongToFile(CURRENT_EPOCH_FILENAME, currentEpoch);
            }
```

同理，获取accepted epoch。
``` java
	try {
            	acceptedEpoch = readLongFromFile(ACCEPTED_EPOCH_FILENAME);
            } catch(FileNotFoundException e) {
            	// pick a reasonable epoch number
            	// this should only happen once when moving to a
            	// new code version
            	acceptedEpoch = epochOfZxid;
            	LOG.info(ACCEPTED_EPOCH_FILENAME
            	        + " not found! Creating with a reasonable default of {}. This should only happen when you are upgrading your installation",
            	        acceptedEpoch);
            	writeLongToFile(ACCEPTED_EPOCH_FILENAME, acceptedEpoch);
            }
```

# 启动ServerCnxnFactory线程
ServerCnxnFacotry是管理ServerCnxn处理类的工厂,它负责对connection上数据处理的调度,以及server级别的一些处理,例如关闭指定session等。有关ServerCnxnFactory的实现类，可分为三种情况：

- NIO模式。这是Zookeeper默认的ServerCnxnFactory实现，其实现类为NIOServerCnxnFactory。
- Netty模式。在Zookeeper 3.4以后引入了Netty作为Server端连接处理的可选实现。Netty是一套非常高效的异步通信框架。可以通过JVM参数zookeeper.serverCnxnFactory进行配置。
- 自定义模型。Zookeeper还支持自定类来实现通信，同样可以通过JVM参数zookeeper.serverCnxnFactory进行配置。

``` java
Static public ServerCnxnFactory createFactory() throws IOException {
        String serverCnxnFactoryName =
            System.getProperty(ZOOKEEPER_SERVER_CNXN_FACTORY);
        if (serverCnxnFactoryName == null) {
            serverCnxnFactoryName = NIOServerCnxnFactory.class.getName();
        }
        try {
            return (ServerCnxnFactory) Class.forName(serverCnxnFactoryName)
                                                .newInstance();
        } catch (Exception e) {
            IOException ioe = new IOException("Couldn't instantiate "
                    + serverCnxnFactoryName);
            ioe.initCause(e);
            throw ioe;
        }
    }
```

ServerCnxnFactory的主要职责：
1. 在单机模式下，引导完成Zookeeper Server的实例化。
2. 异步接收Client的IO连接，并维护连接的IO操作，这是ServerCnxnFactory的核心功能。ServerCnxnFacotry本身被设计成一个Thread，在完成初始化工作之后，就开始启动自身线程，在线程run方法中，采用NIO的方式Accept客户端连接，创建一个NIOServerCnxn实例，此实例和普通的NIO设计思路一样，它持有当前连接的Channel句柄和Buffer队列，最终将此NIOServerCnxn放入Factory内部的一个set中，以便此后对链接信息进行查询和操作(比如关闭操作,IO中read和write操作等)。

# Leader选举
Leader的选举算法有三种：

- LeaderElection：LeaderElection采用的是fast paxos算法的一种简单实现，目前该算法在3.4以后已经建议弃用。
- FastLeaderElection：FastLeaderElection是标准的fast paxos的实现，这是目前Zookeeper默认的Leader选举算法。
- AuthFastLeaderElection：AuthFastLeaderElection算法同FastLeaderElection算法基本一致，只是在消息中加入了认证信息，该算法在最新的Zookeeper中也建议弃用。

Leader选举算法分析详见：[Zookeeper源码分析-Zookeeper Leader选举算法](/archives/zookeeper-leader-election.html)

# QuorumPeer启动
完成DataDir，ZKDatabase加载以及Leader选举动作后，QuorumPeer完成初始化及启动工作。

# 参考资料
<http://zookeeper.apache.org/doc/r3.4.6/zookeeperInternals.html>

<http://shift-alt-ctrl.iteye.com/blog/1846507>
