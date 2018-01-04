---
layout: post
title: '编译Mysql时configure: error: No curses/termcap library found 的错误解决方法'
date: 2008-11-2
wordpress_id: 235
categories: [Database, MySQL]
tags: [MySQL]
keywords: "MySQL"
description: 
comments: true
---
OS：Ubuntu 8.04

在编译Mysql时
sudo ./configure

如果出现了以下错误：
checking for tgetent in -ltermcap… no
checking for termcap functions library… configure: error: No curses/termcap library found

说明 curses/termcap 库没有安装

``` bash
sudo apt-cache search curses | grep lib
```
安装 libncurses5-dev ，然后重新运行配置

``` bash
sudo apt-get install libncurses5-dev
```
然后就安装吧：sudo make &amp;&amp; make install
