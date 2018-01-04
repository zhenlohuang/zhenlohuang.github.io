---
layout: post
title: "Zookeeper源码分析-数据模型"
date: 2014-09-14 17:22:15 +0800
categories: [Big Data, ZooKeeper]
tags: [ZooKeeper]
comments: true
---
本文主要介绍Zookeeper的数据模型，包括Zookeeper的数据视图，节点类型以及节点所包含的信息。

# 节点
Zookeeper的数据视图采用的是类似Unix的数据视图，但是并没有引入文件系统的相关概念：目录和文件，而是引入了节点的概念，称为Znode。它是Zookeeper最小的组成单元，每个Znode包含三部分组成：

- **data**：表示节点所存储的数据，单个节点存储数据不能超过1M。
- **stat**：表示节点的信息，例如节点创建时间，节点的版本，节点的权限等信息。
- **children**:表是所包含的子节点。

![zookeeper-data-model](/images/uploads/2014/09/zookeeper-data-model.jpg)

# 节点类型
Zookeeper中的节点类型可以分为三种：

- 持久节点（Perisient Node），这类节点在创建之后将一直存在，知道有客户端对它进行显示删除，也就是说它不会因为创建其的client的关闭而消失。
- 临时节点（Ephemeral Node），这类节点的生命周期与创建它们的client绑定。也就是说当client的session关闭时，其所创建的临时节点也将被删除。这里有一点需要注意的是session关闭，而不是connection loss。因为zookeeper的client支持session转移，也就是当所连接的server出现网络连接断开或者server down掉的情况，client将自动选择zookeeper集群中的其他节点进行连接，同时将session转移到其他节点上。这种情况只能算connection loss，而不会造成session关闭。除非，该client无法连接所有的节点，支持session timeout。
- 时序节点（Sequential Node）,这类节点在创建节点的时候，zookeeper会自动为其添加一个数字后缀%10d，例如/test-0000000001。不过这类节点是不能单独存在的，需要同持久节点或临时节点组合使用形成：
  + Perisient Sequential Node
  + Ephemeral Sequential Node

# 节点信息
Znode的信息包含下面一些字段：
<table>
<tr>
<td>czxid</td>
<td>创建这个znode的zxid</td>
</tr>
<tr>
<td>mzxid</td>
<td>最后一次修改这个znode的zxid</td>
</tr>
<tr>
<td>ctime</td>
<td>该znode创建时间</td>
</tr>
<tr>
<td>mtime</td>
<td>该znode最后一次修改的时间</td>
</tr>
<tr>
<td>version</td>
<td>该znode的version，也就是该znode的修改次数。</td>
</tr>
<tr>
<td>cversion</td>
<td>该znode的子节点的version</td>
</tr>
<tr>
<td>aversion</td>
<td>该znode的ACL信息version</td>
</tr>
<tr>
<td>ephemeralOwner</td>
<td>如果该znode是ephemeral node，此字段就是对应client的session；否则为0。</td>
</tr>
<tr>
<td>dataLength</td>
<td>The length of the data field of this znode.</td>
</tr>
<tr>
<td>numChildren</td>
<td>子节点个数</td>
</tr>
</table>
Note:zxid = znode transaction id

查看节点信息，可以使用zkCli中的get命令。

```
[zk: localhost:2181(CONNECTED) 15] get /zk_test
my_data
cZxid = 0x8
ctime = Sun Sep 14 14:43:56 CST 2014
mZxid = 0x8
mtime = Sun Sep 14 14:43:56 CST 2014
pZxid = 0x10
cversion = 2
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 7
numChildren = 2
```
