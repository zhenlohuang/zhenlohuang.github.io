---
layout: post
title: 'Python中int()函数的用法'
date: 2010-5-11
wordpress_id: 423
permalink: /archives/the-use-of-python-int-function.html
categories: [Programming]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
int()是Python的一个内部函数

Python系统帮助里面是这么说的

``` python 
>>> help(int)
Help on class int in module __builtin__:
class int(object)
 |  int(x[, base]) -> integer
 |
 |  Convert a string or number to an integer, if possible.  A floating point
 |  argument will be truncated towards zero (this does not include a string
 |  representation of a floating point number!)  When converting a string, use
 |  the optional base.  It is an error to supply a base when converting a
 |  non-string.  If base is zero, the proper base is guessed based on the
 |  string content.  If the argument is outside the integer range a
 |  long object will be returned instead.
```

``` python
>>> int(12.0)
12
```

int()函数可以将一个数转化为整数

``` python
>>> int('12',16)
18
```

这里有两个地方要注意：1）12要以字符串的形式进行输入，如果是带参数base的话

2）这里并不是将12转换为16进制的数，而是说12就是一个16进制的数，int()函数将其用十进制数表示，如下

``` python 
>>> int('0xa',16)
10
>>> int('10',8)
8
```
