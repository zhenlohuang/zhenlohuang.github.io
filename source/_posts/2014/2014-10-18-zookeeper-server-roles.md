---
layout: post
title: "Zookeeper源码分析-Zookeeper角色"
date: 2014-10-18 20:50:59 +0800
permalink: /archives/zookeeper-server-roles.html
categories: [Distributed System]
tags: [ZooKeeper]
comments: true
---
在Zookeeper集群中，主要分为三者角色，而每一个节点同时只能扮演一种角色，这三种角色分别是：

+ Leader：接受所有Follower的提案请求并统一协调发起提案的投票，负责与所有的Follower进行内部的数据交换(同步);
+ Follower：直接为客户端服务并参与提案的投票，同时与Leader进行数据交换(同步);
+ Observer：直接为客户端服务但并不参与提案的投票，同时也与Leader进行数据交换(同步);

Follower与Observer并称为Learner。

![zookeeper-server-roles](/images/uploads/2014/10/zookeeper-server-roles.png)

# Leader
在Zookeeper集群中，只有一个Leader节点，其主要职责：

+ 恢复数据；
+ 维持与Learner的心跳，接收Learner请求并判断Learner的请求消息类型； 

Leader的工作流程简图如下所示，在实际实现中，流程要比下图复杂得多，启动了三个线程来实现功能。

![leader-workflow](/images/uploads/2014/10/leader-workflow.jpg)

PING：Learner的心跳。  
REQUEST：Follower发送的提议信息，包括写请求及同步请求。   
ACK：Follower的对提议的回复，超过半数的Follower通过，则commit该提议。  
REVALIDATE：用来延长SESSION有效时间。

# Follower
在Zookeeper集群中，follower可以为多个，其主要职责：

+ 向Leader发送请求；
+ 接收Leader的消息并进行处理；
+ 接收Zookeeper Client的请求，如果为写清求，转发给Leader进行处理

Follower的工作流程简图如下所示，在实际实现中，Follower是通过5个线程来实现功能的。

![follower-workflow](/images/uploads/2014/10/follower-workflow.jpg)

PING：心跳消息。  
PROPOSAL：Leader发起的提案，要求Follower投票。  
COMMIT：服务器端最新一次提案的信息。  
UPTODATE：表明同步完成。  
REVALIDATE：根据Leader的REVALIDATE结果，关闭待revalidate的session还是允许其接受消息。  
SYNC：返回SYNC结果到客户端，这个消息最初由客户端发起，用来强制得到最新的更新。

# Observer
此处Observer就不再分析，其与follower基本相同，唯一不同就在于Observer不参与投票。
