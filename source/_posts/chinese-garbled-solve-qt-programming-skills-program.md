---
layout: post
title: 'Qt编程技巧  程序中文乱码解决'
date: 2009-10-26
wordpress_id: 359
categories: [Programming]
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

