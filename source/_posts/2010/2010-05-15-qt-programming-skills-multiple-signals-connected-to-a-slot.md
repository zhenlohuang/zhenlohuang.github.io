---
layout: post
title: 'Qt编程技巧  多个信号连接一个槽'
date: 2010-5-15
wordpress_id: 424
permalink: /archives/qt-programming-skills-multiple-signals-connected-to-a-slot.html
categories: [Programming]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
多个signals连接一个slot的时候，可以使用QObject::sender()函数进行读取所产生的对象，之后只要加一个强制类型转换就ok了

实例如下：

连接部分：

``` cpp 
    connect(ui->button0, SIGNAL(clicked()), this, SLOT(append()));
    connect(ui->button1, SIGNAL(clicked()), this, SLOT(append()));
```
slot实现部份：

``` python 
    QObject *object = QObject::sender();
    QPushButton *sender = qobject_cast<QPushButton *>(object);
```
