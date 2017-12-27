---
layout: post
title: 'shell学习笔记四  循环'
date: 2009-9-27
wordpress_id: 346
categories: [Linux]
tags: [Shell]
keywords: "Shell"
description: 
comments: true
---
shell常见的循环语句有for循环、while循环、until循环

for 循环
语法：for 变量 in 列表
do
操作
done
注：变量是要在循环内部用来指代当前所指代的列表中的那个对象的。
列表是在for 循环的内部要操作的对象，可以是字符串也可以是文件，如果是文件则为文件名。

While循环
语法：while 表达式
do
操作
done
只要while表达式成立，do和done之间的操作就一直会进行。

until循环
语法：until 表达式
do
操作
done
重复do和done之间的操作直到表达式成立为止。

``` bash
#!/bin/sh
i=1
while [ $i -lt 20 ]
do
	echo $i
	i=$(($i+1))
#	i= $i+1 是错的
done
 ```