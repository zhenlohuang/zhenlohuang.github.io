---
layout: post
title: '解决netbeans 安装乱码问题'
date: 2009-2-13
wordpress_id: 258
categories: [Programming]
tags: [Netbeans]
keywords: "Netbeans"
description: 
comments: true
---
测试环境：Ubuntu 8.10

软件：Netbeans 6.5

在Linux操作系统中安装netbeans中文版出现了乱码，显示为一些方框。
这个问题不是netbeans 的问题而是JDK的中文显示出了问题...

解决方法是把/usr/share/fonts/truetype/arphic/下的字体复制到JAVA_HOME/jre/lib/fonts/fallback下面(如果没有此目录 新建)。

其中JAVA_HOME是jdk安装的路径，请自己调整。
