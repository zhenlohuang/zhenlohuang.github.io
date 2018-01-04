---
layout: post
title: 'Qt4程序中文乱码解决方案'
date: 2009-7-18
wordpress_id: 330
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
这个主要是编码的问题，我用的Ubuntu，貌似XP就没有这个问题，蛮发下....

只要在程序种加上这两句：

``` cpp
QTextCodec::setCodecForCStrings(QTextCodec::codecForLocale());
 QTextCodec::setCodecForTr(QTextCodec::codecForName("utf8"));
```
具体编码要看具体环境了，可能是GBK或者GB18030或者其他的.....


PS:今天才搞懂，BS自己阿....
