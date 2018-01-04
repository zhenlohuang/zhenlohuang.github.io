---
layout: post
title: 'Python学习笔记之二 文本文件合并'
date: 2010-5-9
wordpress_id: 422
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
今天写的这个文本文件合并，基本上是把昨天写的那个两个脚本合并起来了，详见代码

``` python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
'mergeTextFile--用于合并两个文本文件'
import os
#输入两个文件名
while True:
    file1 = raw_input('输入要合并的第一个文件名：')
    if not os.path.exists(file1):
        print '文件不存在'
        continue
    else :
        break
    
while True:
    file2 = raw_input('输入要合并的第二个文件名：')
    if not os.path.exists(file2):
        print '文件不存在'
        continue
    else :
        break
#打开两个文件
contents = []
try :
    fobj = open(file1, 'r')
except IOError, error :
    print ' %s 打开失败:%s'  % (file1,error)
else :
    for eachline in fobj :
        print eachline
        contents.append(eachline)
fobj.close()
try :
    fobj = open(file2, 'r')
except IOError, error :
    print ' %s 打开失败:%s'  % (file2,error)
else :
    for eachline in fobj :
        print eachline
        contents.append(eachline)
fobj.close()
#创建合并文件
while True:
    merge = raw_input('输入要合并后新文件名：')
    if os.path.exists(merge):
        print '文件已存在'
        continue
    else :
        break
fobj = open(merge, 'w')
fobj.writelines(['%s%s' % (eachline, os.linesep) for eachline in contents])
fobj.close()
```
