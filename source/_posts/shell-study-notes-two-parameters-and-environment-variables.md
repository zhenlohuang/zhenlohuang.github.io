---
layout: post
title: 'shell学习笔记二  参数和环境变量'
date: 2009-9-26
categories: [Linux]
tags: [Shell]
keywords: "Shell"
description: 
comments: true
---
这次要学习一下，参数和环境变量

``` bash
#!/bin/sh
echo "The program's name is $0"
echo "The first parameter is $1"
echo "The second parameter is $2"
echo "The parameter list is $*"
echo "The user's home directory is $HOME"
exit 0 
```

在终端中输入：./ex_02.sh hello killua

执行结果：

``` bash
The program's name is ./ex_02.sh
The first parameter is hello
The second parameter is killua
The parameter list is hello killua
The user's home directory is /home/killua 
```

PS:主要环境变量都是大写的，写$Home 程序不会报错，但是没有结果
