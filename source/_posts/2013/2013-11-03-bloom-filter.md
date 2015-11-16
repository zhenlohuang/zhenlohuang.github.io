---
layout: post
title: '华丽的数据结构之Bloom Filter'
date: 2013-11-3
wordpress_id: 4589
permalink: /archives/bloom-filter.html
categories: [Algorithm]
tags: []
keywords: ""
description: 
comments: true
---
# Bloom Filter简介
Bloom Filter（中文译作：布隆过滤器）是1970年由Bloom提出的。它实际上是由一个很长的二进制位数组和一系列Hash函数组成。

Bloom Filter是一种空间效率很高的随机数据结构，它利用位数组很简洁地表示一个集合，并能判断一个元素是否属于这个集合。Bloom Filter的这种高效是有一定代价的：在判断一个元素是否属于某个集合时，有可能会把不属于这个集合的元素误认为属于这个集合（false positive）。因此，Bloom Filter不适合那些“零错误”的应用场合。而在能容忍低错误率的应用场合下，Bloom Filter通过极少的错误换取了存储空间的极大节省。

# Bloom Filter算法
创建一个m位BitSet，先将所有位初始化为0，然后选择k个独立的哈希函数。第i个哈希函数对字符串str哈希的结果记为hash,,i,,(str)，且i的范围是0到m-1 。有关Bloom Filter的操作的过程可以参考<http://billmill.org/bloomfilter-tutorial/>

## 添加字符串
对于字符串str，分别计算hash,,1,,(str)，hash,,2,,(str)…… hash,,k,,(str)。然后将BitSet的第hash,,1,,(str)，hash,,2,,(str)…… hash,,k,,(str)位设为1。就此，就完成了将字符串str映射到BitSet中的k个二进制位。

![image](/images/uploads/2013/11/649px-Bloom_filter.svg_.png)

## 查找字符串

下面是检索字符串str是否在BitSet中：

对于字符串str，分别计算hash,,1,,(str)，hash,,2,,(str)…… hash,,k,,(str)。然后检查BitSet的第hash,,1,,(str)，hash,,2,,(str)…… hash,,k,,(str)位是否全为1。

若一个字符串对应的bit位不全为1，则可以肯定该字符串一定没有被Bloom Filter记录过。

但是若一个字符串对应的Bit全为1，实际上是不能100%的肯定该字符串被Bloom Filter记录过的。因为有可能该字符串的所有位都刚好是被其他字符串所对应，这种将该字符串划分错的情况，称为false positive 。

## 删除字符串

在基本的Bloom Filter中，字符串加入了就被不能删除了，因为删除会影响到其他字符串。实在需要删除字符串的可以使用<a title="http://en.wikipedia.org/wiki/Bloom_filter#Counting_filters" href="http://en.wikipedia.org/wiki/Bloom_filter#Counting_filters">Counting Bloom Filter(CBF)</a>，这是一种基本Bloom Filter的改进，CBF将基本Bloom Filter每一个Bit改为一个计数器，这样就可以实现删除字符串的功能了。

# Bloom Filter参数选择

## hash函数选择

hash函数的选择对性能影响比较大，一个优秀的hash函数应该做到将字符串等概率的映射到各个bit。与此同时，选择k个不同的hash比较繁琐，一种简单的策略就是采用同一个hash函数然后传入k不同的参数。下面列举的是一些软件框架中BloomFilter所使用的hash函数。
- Cassandra 使用的*Murmur hashes*
- Hadoop 默认使用 *Jenkins and Murmur hashes*
- python-bloomfilter 使用*cryptographic hashes*
- Plan9 使用*Mitzenmacher 2005*
- Sdroege Bloom filter 使用*fnv*
- Squid 使用*MD5*

## Bloom Filter参数*

哈希函数个数k、位数组大小m、加入的字符串数量n的关系可以参考<http://pages.cs.wisc.edu/~cao/papers/summary-cache/node8.html>。该文献证明了对于给定的m、n，当

![image](/images/uploads/2013/11/img11.gif)

时出错的概率是最小的。

同时参考文献中也给出了false positive概率与m、n的关系。false postive概率等于

![image](/images/uploads/2013/11/img10.gif)

完整的参数关系推导请参考<http://pages.cs.wisc.edu/~cao/papers/summary-cache/node8.html>

# Bloom Filter vs HashMap
Bloom Filter跟HashMap不同之处在于：Bloom Filter使用了k个哈希函数，每个字符串跟k个bit对应，从而降低了冲突的概率。

# Bloom Filter代码实现
GitHub: <https://github.com/kevinxhuang/BloomFilter">https://github.com/kevinxhuang/BloomFilter>

# 参考资料
布隆过滤器: <http://zh.wikipedia.org/wiki/%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8">

BloomFilter——大规模数据处理利器: <http://www.cnblogs.com/heaad/archive/2011/01/02/1924195.html>

Bloom Filter概念和原理: <http://blog.csdn.net/jiaomeng/article/details/1495500>