---
layout: post
title: 'Ubuntu 9.04 wubi安装'
date: 2009-5-21
wordpress_id: 291
permalink: /archives/ubuntu-904-wubi-installation.html
categories: [Linux]
tags: [Ubuntu]
keywords: "Ubuntu"
description: 
comments: true
---
本来是将硬盘分成两块，然后装双系统的。可是最近grub老师挂掉，然后就又要修复xp的启动实在太麻烦了

所以改用wubi来装，可是安装的过程中出现一直停留在creating the virual disks那里，无法进行下去。

解决方法如下：这个问题是由于文件系统造成的，本来我是用fat32的，不能创建虚拟文件系统，后来格式化为ntfs就可以了。（这个难道跟MS那次fat专利起诉案由关？？）

顺便补充下：要是系统有装ghost的，最好先删除，不然可能会出现启动时候会有乱码问题，应是说是根本进不去…..貌似8.04有这个问题，不知道9.04有没