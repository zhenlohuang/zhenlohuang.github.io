---
layout: post
title: 'Python学习  随机数据生成器'
date: 2010-8-5
wordpress_id: 457
permalink: /archives/learning-random-data-generator-in-python.html
categories: [Programming]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
这里主要使用了一个random随机模块中的randint和choice。Python的随机模块还是很强大的。

``` python 
#!/usr/bin/env python
#随机数据生成
from random import randint,choice
from string import lowercase
from sys import maxint
from time import ctime
def randDataGenerate () :
    doms = ('sina.com', '163.com', 'cctv.cn', 'yahoo.com.cn', 'csdb.net')
    
    for i in range(randint(5, 20)) :
                   #随机日期生成
                   dateInt = randint(0, maxint - 1)
                   dateStr = ctime(dateInt)
                   #随机E-Mail地址生成
                   email = ''
                   for j in range(randint(4, 7)) :
                       email += choice(lowercase)
                   email += '@' + choice(doms)
                   #随机数生成
                   num = randint(0, 1000)
                   print '%s >> %s :: %d' % (dateStr, email, num)
def main() :
    randDataGenerate()
if __name__ == '__main__' :
    main() 
```
