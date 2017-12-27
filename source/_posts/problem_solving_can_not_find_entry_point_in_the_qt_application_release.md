---
layout: post
title: '关于Qt 程序Release后不能找到输入点的问题解决'
date: 2010-4-19
wordpress_id: 416
categories: [Programming]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---

程序在Qt Creator的Release下运行得好好的，可是一拿出来就不行了，之后我也拷了相应的库还是不行，加了环境变量也不行。提示说：程序找不到输入点 XXX QtCore4.dll。

解决方法：
库是肯定要拷的，关键是考哪个库的问题了，QtCore和QtGui主要是这两个，这两个要拷Qt/qt/bin目录下面的，因为Qt/bin下面也有，所以这个地方要注意，要拷的库不正确就会出现这个问题。

下面这些库是windows下发布Qt必须的：
mingwm10.dll
libgcc_s_dw2-1.dll
QtGui4.dll
QtCore4.dll

其他的自己添加就ok了
