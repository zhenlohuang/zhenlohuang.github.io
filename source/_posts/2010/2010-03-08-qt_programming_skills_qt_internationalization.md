---
layout: post
title: 'Qt编程技巧  Qt 国际化'
date: 2010-3-8
wordpress_id: 406
permalink: /archives/qt_programming_skills_qt_internationalization.html
categories: [Programming]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---


1）首先要修改XX.pro工程文件，加入一句 TRANSLATIONS += XXX.ts

2）然后在终端中运行lupdate XX.pro 生成ts文件

3）然后用Qt Linguist 翻译

4）Qt Linguist里面有个发布功能，生成一个.qm的文件

