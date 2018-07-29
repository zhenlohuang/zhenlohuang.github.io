---
layout: post
title: 'QtSql：没有该文件或目录 解决方案'
date: 2009-7-31
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
显然include <QtSql>是没错的，可是怎么也找不到这个文件，程序编译不过.....

解决方案如下：

在XXX.pro文件中添加一行

```
QT += sql
```
就可以了，这个方法还可以用再其他场合...要是找不到什么头文件可以考虑试试
