---
layout: post
title: '虚拟机Host-Only网络配置'
date: 2013-7-29
wordpress_id: 4057
categories: [Others]
tags: [Virtual]
keywords: "Virtual"
description: 
comments: true
---
主机系统：Windows 7 Professional

虚拟系统：Oracle Linux 6

虚拟机：VirtualBox

步骤：

1）开启主机网卡网络共享

![image](/images/uploads/2013/07/1.png)

2）虚拟机选择网卡

![image](/images/uploads/2013/07/2.png)

3）虚拟机内系统网卡配置

此处Gateway：192.168.137.1，为VirtualBox Host-Only Ethernet Adapter的IP地址，此处DNS是必须的，如果不加DNS将无法访问外网。

![image](/images/uploads/2013/07/QQ截图20130729223614.png)