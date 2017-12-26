---
layout: post
title: 'Qt编程技巧  QLCDNumber与QSpinBox链接'
date: 2009-10-26
wordpress_id: 354
permalink: /archives/qt-programming-skills-qlcdnumber-and-qspinbox-link.html
categories: [Programming]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---

```cpp 
connect(spinBox, SIGNAL(valueChanged(QString)), lcdNumber, SLOT(display(QString)));
```

