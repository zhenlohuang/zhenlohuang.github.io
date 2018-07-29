---
layout: post
title: 'Qt编程技巧  QFileDialog自动添文件后缀的方法'
date: 2009-10-26
categories: [Programming, C++]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---

``` cpp 
QString filename;
filename = QFileDialog::getSaveFileName(this, tr("保存图片"),QDir::currentPath(), tr("Images (*.png *.bmp *.jpg)"));
if (filename.isNull())
   return ;
if (QFileInfo(filename).suffix().isEmpty())  //若后缀为空自动添加png后缀
    filename.append(".png");
```
