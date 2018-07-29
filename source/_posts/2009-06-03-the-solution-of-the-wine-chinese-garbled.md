---
layout: post
title: 'wine中文乱码的解决方法'
date: 2009-6-3
categories: [Linux]
tags: [wine]
keywords: "wine"
description: 
comments: true
---

操作系统：Ubuntu 9.04

```
sudo gedit ~/.wine/system.reg

搜索： LogPixels
找到的行应该是：[System//CurrentControlSet//Hardware Profiles//Current//Software//Fonts]
将其中的：
"LogPixels"=dword:00000060
改为：
"LogPixels"=dword:00000070

搜索： FontSubstitutes
找到的行应该是：[Software//Microsoft//Windows NT//CurrentVersion//FontSubstitutes]
将其中的：
"MS Shell Dlg"="Tahoma"
"MS Shell Dlg 2&Prime;="Tahoma"
改为：
"MS Shell Dlg"="SimSun"
"MS Shell Dlg 2&Prime;="SimSun"

保存,退出。
