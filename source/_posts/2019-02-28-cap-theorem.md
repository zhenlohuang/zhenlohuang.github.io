---
title: 分布式系统 - CAP定理
tags: Distributed System
categories: Distributed System
date: 2019-02-28 22:57:27
---


# CAP定理简介
在理论计算机科学中，CAP定理（CAP theorem），又被称作布鲁尔定理（Brewer’s theorem），指的是在一个分布式系统中，Consistency（一致性）、 Availability（可用性）、Partition tolerance（分区容错性）这三个基本需求，最多只能同时满足其中的2个
* 一致性（Consistency）：数据在多个副本之间能够保持一致的特性
* 可用性（Availability）：系统提供的服务必须一直处于可用的状态，每次请求都能获取到非错的响应（不保证获取的数据为最新数据）
* 分区容错性（Partition tolerance）：分布式系统在遇到任何网络分区故障的时候，仍然能够对外提供满足一致性和可用性的服务，除非整个网络环境都发生了故障

# CAP权衡
既然根据CAP定理，我们无法同时满足一致性，可用性和分区容错性，那要舍弃哪个呢？
* CA without P
如果不要求P（不允许分区），则C（强一致性）和A（可用性）是可以保证的。但其实分区不是你想不想的问题，而是始终会存在，因此CA的系统更多的是允许分区后各子系统依然保持CA。
* CP without A
如果不要求A（可用性），相当于每个请求都需要在Server之间强一致，而P（分区）会导致同步时间无限延长，如此CP也是可以保证的。很多传统的数据库分布式事务都属于这种模式。
* AP wihtout C
要高可用并允许分区，则需放弃一致性。一旦分区发生，节点之间可能会失去联系，为了高可用，每个节点只能用本地数据提供服务，而这样会导致全局数据的不一致性。实际上，目前大部分NoSQL都属于这一类。

# 参考文档
* https://en.wikipedia.org/wiki/CAP_theorem