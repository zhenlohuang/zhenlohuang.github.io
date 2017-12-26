---
layout: post
title: 'Ubuntu下 mp3信息乱码问题解决'
date: 2009-5-22
wordpress_id: 295
permalink: /archives/mp3-information-garbled-problem-solving-in-ubuntu.html
categories: [Others]
tags: []
keywords: ""
description: 
comments: true
---
系统：Ubuntu 9.04

# 方案一：

需要有软件包mid3iconv

```
sudo apt-get install python-mutagen
```
进入mp3目录在终端输入

```
mid3iconv -e GBK *.mp3 
mid3iconv -e GBK */*.mp3 #如果包含子目录
```

# 方案二：

下载一个easytag

```
sudo apt-get install easytag
```
在设置-&gt;首选项里面修改设置，将读取ID3标签使用的字符集和ID3v1修改为GB2312
