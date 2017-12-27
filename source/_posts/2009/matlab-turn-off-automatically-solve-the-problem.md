---
layout: post
title: 'matlab自动关闭问题解决'
date: 2009-3-11
wordpress_id: 267
categories: [Programming]
tags: [Matlab]
keywords: "Matlab"
description: 
comments: true
---
这是由于AMD的CPU的问题，貌似Intel 的没有这个问题，只要在系统的环境变量中添加对AMD支持就可以了.

1、右击我的电脑,选择属性.

2、在"高级"选项卡中点击"环境变量"
3、在系统变量下面添加如下内容(按"新建"):

变量名:BLAS_VERSION
变量值:X:/Matlab7/bin/win32/atlas_Athlon.dll

"X"为您安装MATLAB的盘符,确定后即可
