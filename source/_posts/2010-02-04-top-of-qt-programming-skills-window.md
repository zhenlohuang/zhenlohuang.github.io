---
layout: post
title: 'Qt编程技巧  窗口置顶'
date: 2010-2-4
categories: [Programming, C++]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---
一般来是说窗体置顶和取消只要

``` cpp
setWindowFlags(Qt::WindowStaysOnTopHint);
         setWindowFlags(Qt::Widget); 
```

要是开始不设置这个，后面要再设置就不可以了所以要加以改进，可以先hide(),然后在show()，代码如下：

``` cpp
hide();
setWindowFlags(Qt::WindowStaysOnTopHint);
show();

hide();
setWindowFlags(Qt::Widget);
show(); 
```
