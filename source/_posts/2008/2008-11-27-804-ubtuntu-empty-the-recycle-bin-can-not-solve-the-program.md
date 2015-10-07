---
layout: post
title: 'Ubtuntu 8.04回收站清空不了解决方案'
date: 2008-11-27
wordpress_id: 247
permalink: /archives/804-ubtuntu-empty-the-recycle-bin-can-not-solve-the-program.html
categories: [Linux]
tags: [Ubuntu]
keywords: "Ubuntu"
description: 
comments: true
---
这条命令可以用来清空回收站的东西。。

``` bash
sudo rm -fr $HOME/.local/share/Trash/files/*
```