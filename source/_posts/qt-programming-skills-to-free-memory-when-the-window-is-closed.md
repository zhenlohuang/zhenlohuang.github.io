---
layout: post
title: 'Qt编程技巧  窗口关闭时释放内存'
date: 2010-1-20
wordpress_id: 378
categories: [Programming, C++]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---
当用户关闭也窗口时，其默认行为是隐藏，所以还会保留在内存中，解决方法是在构造函数中加入这个一句

```
setAttribute(Qt::WA_DeleteOnClose); 
```
Qt::WA_DeleteOnClose属性是可以在QWidget上进行设置并影响这个窗口部件的行为的标记之一
