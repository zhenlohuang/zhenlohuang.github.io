---
layout: post
title: 'Oracle查询结果自动生成序号'
date: 2011-11-27
wordpress_id: 524
categories: [Database]
tags: [Oracle]
keywords: "Oracle"
description: 
comments: true
---
一般情况下，可以如下：

``` sql
select rownum, a from A;
```

但是当后面有多表关联，order by排序的时候，

``` sql
select rownum, a from A，B where A.a=B.b order by A.a;
```
rownum就可能会乱了。

这时候，可以利用分析函数rank()来实现：

``` sql
select rank() over(order by t.b) rowno, t.a, t.c from test t order by t.b;
```
这样就既可以排序，又可以自动加上连续的序号了。

参考资料：
<http://yuaoi.iteye.com/blog/767889>
<http://www.cnblogs.com/mycoding/archive/2010/05/29/1747065.html>
