---
layout: post
title: 'Qt编程技巧  菜单栏多选项问题'
date: 2010-2-4
wordpress_id: 386
categories: [Programming]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---

菜单多选有两种，一种是单选，一种是多选
多选简单，只要将Action，setCheckable(true)

单选的话，也要将Action，setCheckable(true)，之后还要建立一个QActionGroup，将Action都加进去就ok了
