---
layout: post
title: 'VirtualBox  虚拟机主机时间不同步设置'
date: 2011-9-1
categories: [Others]
tags: [VirtualBox]
keywords: "VirtualBox"
description: 
comments: true
---

用dos进入到VirtualBox的安装目录下，找到VBoxManage.exe

执行：

```
VBoxManage.exe setextradata WinXP_JP "VBoxInternal/Devices/VMMDev/0/Config/GetHostTimeDisabled" "1″
```