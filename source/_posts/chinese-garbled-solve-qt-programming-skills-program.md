---
layout: post
title: 'Qt编程技巧  程序中文乱码解决'
date: 2009-10-26
categories: [Programming, C++]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---
加上这两句，修改程序的编码方式

``` cpp 
QTextCodec::setCodecForCStrings(QTextCodec::codecForLocale());
QTextCodec::setCodecForTr(QTextCodec::codecForName("utf8")); 
```

