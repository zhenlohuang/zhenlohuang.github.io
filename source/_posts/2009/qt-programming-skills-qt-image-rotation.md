---
layout: post
title: 'Qt编程技巧  Qt图片翻转'
date: 2009-10-26
wordpress_id: 356
categories: [Programming]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---

``` cpp 
/**
  水平翻转
  */
void ImageViewer::horFilp()
{
    image = image.mirrored(true, false);
    imageLabel->setPixmap(QPixmap::fromImage(image));
}
/**
  垂直翻转
  */
void ImageViewer::verFilp()
{
    image = image.mirrored(false, true);
    imageLabel->setPixmap(QPixmap::fromImage(image));
}
/**
  顺时针旋转
  */
void ImageViewer::clockwise()
{
    QMatrix matrix;
    matrix.rotate(90.0);
    image = image.transformed(matrix,Qt::FastTransformation);
    imageLabel->setPixmap(QPixmap::fromImage(image));
}
/**
  逆时针旋转
  */
void ImageViewer::anticlockwise()
{
    QMatrix matrix;
    matrix.rotate(-90.0);
    image = image.transformed(matrix,Qt::FastTransformation);
    imageLabel->setPixmap(QPixmap::fromImage(image));
}
```

