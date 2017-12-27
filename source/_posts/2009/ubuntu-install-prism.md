---
layout: post
title: 'Ubuntu 下安装Prism'
date: 2009-10-6
wordpress_id: 349
categories: [Others]
tags: [Prism]
keywords: "Prism"
description: 
comments: true
---
Prism 的前身就是我们先前所介绍的 WebRunner。WebRunner 现在已经是 Mozilla Labs 中的一个项目，并更名成了 Prism。Prism 支持直接为 Web 应用程序创建快捷方式，允许 Web 应用程序处理特定的内容类型，为 Web 应用程序上传文件时支持拖拉操作，支持运行离线应用等等。

如果你使用 Ubuntu 9.04 ，源里就有 Prism ，只不过版本要旧一点，为了获得最好的聊天体验，我建议你安装最新版本。为了安装方便，请添加 PPA for Ubuntu Mozilla Daily Build Team 的第三方源，编辑 /etc/apt/sources.list 文件，并加入下列两行：

```
## PPA for Ubuntu Mozilla Daily Build Team
deb http://ppa.launchpad.net/ubuntu-mozilla-daily/ppa/ubuntu jaunty main
deb-src http://ppa.launchpad.net/ubuntu-mozilla-daily/ppa/ubuntu jaunty main 
```

保存，然后依次执行下列命令安装即可：

```
sudo apt-key adv –keyserver keyserver.ubuntu.com –recv-keys 247510BE
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install prism
```
