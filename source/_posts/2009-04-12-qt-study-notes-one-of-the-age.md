---
layout: post
title: 'QT学习笔记之零 Hello World'
date: 2009-4-12
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
之所以从零开始，这个也是C++的习惯吧，第一个QT程序啊，纪念一下，还是经典的Hello World

main.cpp

``` cpp
#include <QtGui/QApplication>
#include<QLabel>
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    QLabel *label=new QLabel("Hello World");
    label->show();
    return a.exec();
}
```
运行结果：

![image](/images/legacy/2009/04/223451108.p.JPG)

PS:入门第一件事情果然是Hello World