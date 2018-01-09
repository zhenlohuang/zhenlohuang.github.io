---
layout: post
title: 'Python Challenge攻略之Level 4'
date: 2013-5-10
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
# 攻略：
首先根据提示从linkedlist.html转到linkedlist.php，进入level4    
点击图片得到链接：http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=12345    
网页内容为：and the next nothing is 44827    
将44827替换12345得到网页链接：http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=44827    
得到网页内容：and the next nothing is 45439    
由此得知本关目的就在于利用程序循环重复以上过程直至得到结果。    
中间可能会中断，可以根据提示修正。    

# 代码：

``` python 
#!/usr/bin/env/python
#coding:utf8
'''
Created on May 2, 2013

@author: killua
@url: http://www.pythonchallenge.com/pc/def/equality.html
@target: http://www.pythonchallenge.com/pc/def/linkedlist.html
'''

import urllib
import re

def next_page(nothing_no):
    try:
        text = urllib.urlopen('http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=%s' % nothing_no).read()
        print text
        m = re.match(r'[^\d]*(\d+)$', text)
        return m.groups()[0]
    except:
        return 'END'
    
if __name__ == '__main__':
    
    nothing_no = 12345
    #nothing_no = 16044 /2
    #nothing_no = 63579
    while True:
        nothing_no = next_page(nothing_no)
        if nothing_no.isdigit():
            continue
        else:
            break
```
# 完整代码：
Github：<https://github.com/kevinxhuang/python-challenge>
