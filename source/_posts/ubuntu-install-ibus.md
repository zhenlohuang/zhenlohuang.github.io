---
layout: post
title: 'Ubuntu 安装ibus'
date: 2009-5-27
wordpress_id: 302
categories: [Linux]
tags: [Ubuntu, ibus]
keywords: "Ubuntu, ibus"
description: 
comments: true
---
1.添加ibus源

加入：deb http://ppa.launchpad.net/ibus-dev/ppa/ubuntu jaunty main 这个源即可</span>

sudo apt-get update 更新一下。

然后安装ibus

sudo apt-get install ibus ibus-table ibus-pinyin python-ibus ibus-qt4 ibus-gtk

把这些都装上，前面的三个是必须的，ibus-qt4,ibus-gtk    这两个包可以防止出现不能进行光标跟随的问题

安装之后，im-switch -s ibus切换到ibus，注销一下就可以使用了。

默认不能使用于qt程序中需要修改

sudo gedit /etc/X11/xinit/xinput.d/ibus

添加

```
XIM=ibus
XIM_PROGRAM=/usr/bin/ibus
XIM_ARGS=""
GTK_IM_MODULE=ibus
QT_IM_MODULE=xim
XMODIFIERS="@im=ibus"
DEPENDS="ibus"
```
PS:要是不能启动，先卸载SCIM
