---
layout: post
title: 'Qt编程技巧  右键菜单'
date: 2010-2-4
wordpress_id: 389
categories: [Programming]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---

``` cpp
    QMenu *menu = new QMenu(tr("Right Contex Menu"),this);
    menu->setStyleSheet("background-color : normal");
    menu->addAction(Action1);
    menu->addAction(Action2);
    menu->addAction(Action3);
    menu->addSeparator();
    menu->addAction(Action4);
    menu->exec(event->globalPos()); 
```
