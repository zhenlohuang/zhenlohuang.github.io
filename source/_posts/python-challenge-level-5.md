---
layout: post
title: 'Python Challenge攻略之Level 5'
date: 2013-5-13
wordpress_id: 4030
categories: [Programming]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
# 攻略：
在网页源码中有着这么一段话    

``` html
<peakhell src="banner.p">
<!-- peak hell sounds familiar ? -->
</peakhell>
```
这里包含有两个信息：数据文件banner.p以及pickle模块    
pickle是Python的序列化模块，提供Python对象的序列化与反序列化    

# 代码：

``` python 
#!/usr/bin/env/python
#coding:utf8
'''
Created on May 13, 2013

@author: killua
@url: http://www.pythonchallenge.com/pc/def/peak.html
@target: http://www.pythonchallenge.com/pc/def/channel.html
'''

import urllib
import pickle 

def read_page(url):
    text = urllib.urlopen(url)
    return text

def deserialization(text):
    return pickle.loads(text)
    
if __name__ == '__main__':
    text = open('./input/level05.in').read()
    orgin_objects = deserialization(text)
    
    #draw graph by lists
    for obj in orgin_objects:
        line = ''
        for item in obj:
            line += item[0] * item[1]
        print line
```
# 完整代码：
Github：<https://github.com/kevinxhuang/python-challenge>
