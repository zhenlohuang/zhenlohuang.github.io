---
layout: post
title: 'Qt编程技巧  延时'
date: 2009-10-26
wordpress_id: 362
permalink: /archives/qt-programming-skills-delay.html
categories: [Programming]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---
延时可以使用这个函数

``` cpp 
void QTimer::singleShot ( int msec, QObject * receiver, const char * member )   [static]
```

Example:

```cpp 
 #include <QApplication>
 #include <QTimer>
 int main(int argc, char *argv[])
 {
     QApplication app(argc, argv);
     QTimer::singleShot(600000, &app, SLOT(quit()));
     ...
     return app.exec();
 }
```
