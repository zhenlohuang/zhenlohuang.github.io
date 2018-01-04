---
layout: post
title: 'Qt编程技巧  窗体居中显示'
date: 2009-10-26
wordpress_id: 358
categories: [Programming, C++]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---

``` cpp 
this->resize(150,150);  //窗体大小
//窗体居中
 QDesktopWidget* desktop = QApplication::desktop();
int width = desktop->width();
int height = desktop->height();
move((width - this->width())/2, (height - this->height())/2);
```

PS:resize要放在调整窗体位置前面，不然刚开始的this->width()和this->height()是默认的，再调整就不能保证是居中了
