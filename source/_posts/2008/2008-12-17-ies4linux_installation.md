---
layout: post
title: 'IES4Linux安装'
date: 2008-12-17
wordpress_id: 256
permalink: /archives/ies4linux_installation.html
categories: [Linux]
tags: [Ubuntu]
keywords: "Ubuntu"
description: 
comments: true
---

1）添加源：sudo gedit /etc/apt/sources.list,增加

```
deb http://us.archive.ubuntu.com/ubuntu edgy universe  
deb http://wine.budgetdedicated.com/apt edgy main
```

2）更新源：sudo apt-get update

3）更新wine和cabextract：sudo apt-get install wine cabextract

4)安装python-gtk2-dev:sudo apt-get install python-gtk2-dev

5）下载IEs4Linux wget http://www.tatanka.com.br/ies4linux/downloads/ies4linux-latest.tar.gz

6）安装  
     解压：tar zxvf ies4linux-latest.tar.gz
     运行：./ies4linux
ps：建议选CN，貌似我选了EN不能下载。。