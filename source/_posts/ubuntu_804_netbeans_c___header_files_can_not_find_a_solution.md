---
layout: post
title: 'Ubuntu 8.04 netbeans C++头文件找不到解决办法'
date: 2008-11-24
wordpress_id: 243
categories: [Programming, C++]
tags: [Ubuntu, Netbeans]
keywords: "Ubuntu, Netbeans"
description: 
comments: true
---

有些时候装完netbeans，当你写完一个程序的时候，发现IDE提示说“找不到XXX.h”

解决办法如下：
工具 －》 选项 －》 C/C++ －》 C++编译器  －》 添加 －》/usr/include/c++/4.2.4（你的可能不是4.2.4）