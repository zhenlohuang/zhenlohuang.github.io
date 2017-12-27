---
layout: post
title: 'Qt编程技巧  返回文件列表'
date: 2009-10-26
wordpress_id: 360
categories: [Programming]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---

``` cpp 
QStringList QDir::entryList ( Filters filters = NoFilter, SortFlags sort = NoSort ) const
```

实例:

``` cpp 
    QString path = QFileInfo(fileName).absolutePath();
    QDir dir(path);
    QStringList filters;
    filters<< "*.jpg" << "*.bmp" << "*.png";
    fileList = dir.entryList(filters,QDir::Files,QDir::Name); 
```
