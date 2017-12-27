---
layout: post
title: 'shell学习笔记三  分支结构'
date: 2009-9-26
wordpress_id: 345
categories: [Linux]
tags: [Shell]
keywords: "Shell"
description: 
comments: true
---
分支结构主要是if...else 和case 这两种，下面用简单的练习代码说明下好了

``` bash
#!/bin/sh
echo "Input two number A and B"
read A
read B
echo "A=$A"
echo "B=$B"
if [ $A -gt $B ]; then
	echo "A > B"
elif [ $A -lt $B ]; then
	echo "A < B"
else
       	echo "A = B"
fi
exit 0;
```

运行结果：

``` bash
Input two number A and B
10
5
A=10
B=5
A > B
 ```
 
说到if 肯定要有条件判断，在shell里面条件判断用的是[..] ，还有一点要注意的是then前面的那个分号，要事then和if 在同一行的话就要加上

随便说下，记得以fi结尾

下面说下case结构：

``` bash
#!/bin/sh
echo "Are you OK?:"
read ans
case "$ans" in
	yes | y | Y | Yes | YES ) echo "Yes,I'm OK." ;;
	no | n | N | No | NO ) echo "No,I'm bad." ;;
	* ) echo "Error input."
esac
 ```
 
运行结果：

```
Are you OK?:
yes
Yes,I'm OK.
```

本人一直认为case结构是一个很麻烦的结构，语法复杂又没什么优势，哎.....

注意点主要是条件末尾的）和语句块后面的;;  ，还有case匹配原则是是从上到下的，最后用一个*号，相当与C里面的default吧
