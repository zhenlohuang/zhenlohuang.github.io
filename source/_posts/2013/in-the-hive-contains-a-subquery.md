---
layout: post
title: 'Hive中in包含子查询'
date: 2013-8-11
wordpress_id: 4127
categories: [Big Data]
tags: [Hive]
keywords: "Hive"
description: 
comments: true
---
目前HIVE不支持 not in 中包含查询子句的语法，因此需要使用left semi join或者left outer join来实现这一功能。

SQL:

``` sql
SELECT a.key, a.value FROM a WHERE a.key IN (SELECT b.key FROM B)
SELECT a.key, a.value FROM a WHERE a.key NOT IN (SELECT b.key FROM B)
```
转换为HiveQL的写法:

``` sql
SELECT a.key, a.value FROM a LEFT SEMI JOIN b ON (a.key = b.key)
SELECT a.key, a.value FROM a LEFT OUTER JOIN b ON (a.key = b.key) WHERE b.key1 IS NULL
```
