---
layout: post
title: '一致性Hash算法(Consistent Hashing)'
date: 2013-11-9
wordpress_id: 4606
permalink: /archives/consistent-hashing.html
categories: [Distributed System, Algorithm]
tags: [Hash]
keywords: "Hash"
description: 
comments: true
---
一致性Hash算法（Consistent hashing）早在 1997 年就在论文 [Consistent hashing and random trees](http://portal.acm.org/citation.cfm?id=258660) 中被提出，目前在 cache 系统中应用越来越广泛。

# 应用场景
假设有N 个 cache 服务器，那么如何将一个对象 object 映射到 N 个 cache 服务器上呢，你很可能会采用类似下面的通用方法计算 object 的 hash 值，然后均匀的映射到到 N 个 cache：

``` bash 
hash(object)%N
```
考虑如下的两种情况；    
1）假设一个 cache 服务器m挂掉了，这样所有映射到 cache m 的对象都会失效。这时需要把cache服务器m从系统中移除，cache服务器数变成N-1台，因此映射公式变成：

``` bash 
hash(object)%(N-1)
```
2）如果需要添加cache服务器 ，cache服务器数变成N+1台，映射公式变成：

``` bash 
hash(object)%(N+1)
```
这两种情况的出现，使得原来的cache都失效了。可见上述的Hash算法并不能很好的适应cache系统，因此为了解决这两种情况我们引入一致性Hash。

# 一致性Hash算法
算法具体步骤如下：    
1.    首先求出每个Cache服务器的hash（可以利用IP来计算），并将其配置到一个0~2^32的圆环区间上。    
2.    使用同样的方法求出需要存储对象的hash，也将其配置到这个圆环上。    
3.    从数据映射到的位置开始顺时针查找，将数据保存到找到的第一个Cache节点上。如果超过2^32仍然找不到Cache节点，就会保存到第一个Cache节点上。    
![image](/images/uploads/2013/11/0_1300845930vO032.gif)

## 新增Cache服务器
假设在这个环形哈希空间中，cache5被映射在Cache3和Cache4之间，那么受影响的将仅是沿Cache5逆时针遍历直到下一个Cache（Cache3）之间的对象（它们本来映射到Cache4上）。

![image](/images/uploads/2013/11/0_13008459978RI82.gif)

- 移除Cache服务器

假设在这个环形哈希空间中，Cache3被移除，那么受影响的将仅是沿Cache3逆时针遍历直到下一个Cache（Cache2）之间的对象（它们本来映射到Cache3上）。

![image](/images/uploads/2013/11/0_1300846030mZN31.gif)

# 虚拟Cache服务器
考虑到哈希算法并不是保证绝对的平衡，尤其Cache较少的话，对象并不能被均匀的映射到 Cache上。为了解决这种情况，一致性Hash引入了“虚拟节点”的概念： 一个实际节点对虚拟成若干个“虚拟节点”，这个对应个数也成为“复制个数”，“虚拟节点”在哈希空间中以哈希值排列。仍以4台Cache服务器为例，在下图中看到，引入虚拟节点，并设置“复制个数”为2后，共有8个“虚拟节点”分部在环形区域上，缓解了映射不均的情况。

![image](/images/uploads/2013/11/0_1300846075umFj2.gif)

引入了“虚拟节点”后，映射关系就从【对象--->Cache服务器】转换成了【对象--->虚拟节点---> Cache服务器】。查询对象所在Cache服务器的映射关系如下图所示。

![image](/images/uploads/2013/11/0_1300846198h1782.gif)

# 参考资料
一致性hash算法 - consistent hashing: <http://blog.csdn.net/sparkliang/article/details/5279393>    
一致性哈希算法（Consistent Hashing）: <http://blog.csdn.net/x15594/article/details/6270242>    
一致性hash和solr千万级数据分布式搜索引擎中的应用: <http://blog.jobbole.com/47023/">http://blog.jobbole.com/47023/>    





