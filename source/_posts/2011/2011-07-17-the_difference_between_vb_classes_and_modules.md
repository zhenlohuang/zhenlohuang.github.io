---
layout: post
title: 'VB类与模块的区别'
date: 2011-7-17
wordpress_id: 510
permalink: /archives/the_difference_between_vb_classes_and_modules.html
categories: [Programming]
tags: [VB]
keywords: "VB"
description: 
comments: true
---

类可以实例化为对象，而模块则不能。由于模块的数据只有一个副本，因此当程序的一部分更改模块中的公共变量时，如果程序的其他任何部分随后读取该变量，都会获取同样的值。与之相反，每个实例化对象的对象数据则单独存在。    

类可以被继承，也可以实现接口，模块则不能    

在类中定义的成员其作用范围在类的特定实例内，并且只存在于对象的生存周期内。要从类的外部访问类的成员，必须使用全限名称，格式为Object.Member    