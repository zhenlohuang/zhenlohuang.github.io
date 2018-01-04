---
layout: post
title: 'Python学习  面向对象编程（二）  随机序列迭代器'
date: 2010-8-5
wordpress_id: 452
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
这里代码很简单，实现了一个随机序列迭代器

``` python 
#!/usr/bin/env python
#随机迭代器
from random import choice
class RandSeqIterator(object) :
    def __init__(self, seq) :
        self.data = seq
    def __iter__(self) :
        return self
    def next(self) :
        return choice(self.data)
#Program Test
for eachItem in RandSeqIterator(('AA', 'BB', 'CC', 'DD', 'End')) :
    print eachItem
    if eachItem == 'End' :
        break 
```
