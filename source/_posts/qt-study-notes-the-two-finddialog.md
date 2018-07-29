---
layout: post
title: 'QT学习笔记之一 Age'
date: 2009-4-12
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
主要学习简单的Wdiget的添加问题…

main.cpp

``` cpp
#include <QtGui/QApplication>
#include<QSpinBox>
#include<QSlider>
#include<QHBoxLayout>
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    //创建一个小窗体部件
    QWidget *widget=new QWidget;
    widget->setWindowTitle("input your age");
    //创建一个QSpinBox，并设置范围0-130
    QSpinBox *spinBox=new QSpinBox;
    spinBox->setRange(0,130);
    //创建一个QSlider，并设置范围0-130
    QSlider *slider=new QSlider(Qt::Horizontal);
    slider->setRange(0,130);
    //设置信号和槽
    QObject::connect(spinBox,SIGNAL(vauleChanged(int)),slider,SLOT(setValue(int)));
    QObject::connect(slider,SIGNAL(valueChanged(int)),spinBox,SLOT(setValue(int)));
    //初始值
    spinBox->setValue(0);
    //布局设置
    QHBoxLayout *layout=new QHBoxLayout;
    layout->addWidget(spinBox);
    layout->addWidget(slider);
    widget->setLayout(layout);
    widget->show();
    return a.exec();
}
```
运行结果

![image](/images/legacy/2009/04/223310920.p.jpg?d=20090430223730624)

PS：居然connect中sender的函数打错了还能用….好囧啊