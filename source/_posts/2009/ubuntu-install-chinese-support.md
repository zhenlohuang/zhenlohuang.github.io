---
layout: post
title: 'Ubuntu安装中文支持'
date: 2009-5-22
categories: [Linux]
tags: [Ubuntu]
keywords: ""
description: 
comments: true
---
系统：Ubuntu 9.04

安装中文环境
系统－> 系统管理 －> 语言支持

或者

```
sudo apt-get install xpdf-chinese-simplified xpdf-chinese-traditional poppler-data
```
中文方块问题

某些软件中文字体显示为方块的问题：

```
sudo gedit /etc/fonts/conf.d/49-sansserif.conf
```
找到倒数第4行的 sans-serif 替换为 文泉驿正黑

``` xml
<string>文泉驿正黑</string>
```
