---
layout: post
title: 'Python学习笔记之一 文本文件的创建与显示'
date: 2010-5-8
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
这个程序主要可以实现创建一个文本文件
*makeTextFile.py*

``` python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
'makeTextFile.py--创建一个文本文件'
import os
#输入文件名
while True :
    filename = raw_input('输入文件名')
    if os.path.exists(filename) :
        print 'ERROR: %s already exists' % filename
    else :
        break
    
#输入文件内容
contents = []
print '/n输入每行文本，以#结束'
while True :
    entry = raw_input('> ')
    if entry == '.' :
        break
    else :
        contents.append(entry)
        
#写入文件
fobj = open(filename, 'w')
fobj.writelines(['%s%s' % (eachline, os.linesep) for eachline in contents])
fobj.close()
print 'Done!'
```

这个用来显示
*readTextFile.py*

``` python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
'readTextFile.py--读取显示文本文件'
#获取文件名
filename = raw_input('输入文件名：')
#打开文件
try :
    fobj = open(filename, 'r')
except IOError, error:
    print ' %s 打开失败:%s'  % (filename,error)
    exit()
else :
    #显示文本
    for eachline in fobj :
        print eachline
#文件关闭
fobj.close()
```
