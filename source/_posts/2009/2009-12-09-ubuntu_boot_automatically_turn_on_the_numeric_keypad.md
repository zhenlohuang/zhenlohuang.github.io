---
layout: post
title: 'ubuntu 开机自动开启数字小键盘'
date: 2009-12-9
wordpress_id: 368
permalink: /archives/ubuntu_boot_automatically_turn_on_the_numeric_keypad.html
categories: [Linux]
tags: [Ubuntu]
keywords: "Ubuntu"
description: 
comments: true
---

首先，需要安装一个小软件，在终端中执行以下：

 sudo apt-get install numlockx

然后编辑：

 sudo gedit /etc/gdm/Init/Default

把下面的内容添加到最后那行的前面，（"exit 0"的前面）

 if [ -x /usr/bin/numlockx ]; then
 numlockx on
 fi


搞定了
