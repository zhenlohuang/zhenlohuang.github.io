---
layout: post
title: 'shell学习笔记一  变量'
date: 2009-9-25
wordpress_id: 343
categories: [Linux]
tags: [Shell]
keywords: "Shell"
description: 
comments: true
---
虽然知道shell好久，今天才开始好好学习shell，鄙视下自己，下面都是练习代码写得不好大牛不要鄙视阿....

``` bash
#!/bin/sh
var="Hello World"
echo $var
echo "$var"
echo '$var'
 ```
 
运行结果：
``` bash
Hello World
Hello World
$var
 ```
 
功能很简单就是显示变量，第一个是直接回显，第二个和第三个感觉很相似，还是有很大区别的。如果你把一个带有$字符的变量放在双引号中，就是把它作为变量使用，如果你是放在单引号中就是没有作用，单引号里面可以放一些不含变量的字符串

还有一个点就是权限，chmod +x ex_01.sh

ok
