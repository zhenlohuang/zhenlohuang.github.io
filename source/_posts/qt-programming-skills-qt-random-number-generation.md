---
layout: post
title: 'Qt编程技巧  Qt随机数的产生'
date: 2009-10-26
categories: [Programming, C++]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---
首先调用函数

``` cpp 
qsrand(QTime(0,0,0).secsTo(QTime::currentTime()));
```

产生一个进程, 然后调用函数

``` cpp 
n = qrand();
```

n就是所差生的随机数

