---
layout: post
title: 'Python Challenge攻略之Level 2'
date: 2013-5-10
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
# 攻略：
查看网页源代码，你就可以看到一堆字符，只要在里面找出rare characters（出现次数最少的字符）就可以了。

# 代码：

``` python 
#!/usr/bin/env/python
#coding:utf8
'''
Created on May 2, 2013

@author: killua
@url: http://www.pythonchallenge.com/pc/def/ocr.html
@target: http://www.pythonchallenge.com/pc/def/equality.html
'''

import re

data = open('./input/level02.in').read()

res = re.findall(r"[A-Za-z]", data)
print ''.join(res)
```

# 完整代码：
Github：<https://github.com/zhenlohuang/python-challenge>
