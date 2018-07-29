---
layout: post
title: 'Qt编程技巧  系统托盘设置'
date: 2010-2-4
categories: [Programming, C++]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---

```cpp
    trayIcon = new QSystemTrayIcon(this);   //系统托盘
	
    trayMenu = new QMenu(this);  //托盘菜单
    trayMenu->addAction(Action1);

    ......

    trayMenu->addAction(quitAction10);

    connect(trayIcon, SIGNAL(activated(QSystemTrayIcon::ActivationReason)), this, SLOT(show()));
	
    trayIcon->setIcon(QIcon(":/icons/trayIcon.png"));
    trayIcon->setContextMenu(trayMenu);
```
