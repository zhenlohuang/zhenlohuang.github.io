---
layout: post
title: 'Qt编程技巧  QTextBrowser显示文件内容'
date: 2010-2-20
categories: [Programming, C++]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---
QTextBrowser是一个文本显示类，功能还是很强大的

下面的代码简单的实现了，QTextBrowser显示文本

``` cpp
    QFile file("file.html");
    if(!file.open(QFile::ReadOnly | QFile::Text))
        qDebug() << "Can not open";
    QTextStream in(&file);
    licenceTextBrowser->setHtml(in.readAll());
```
