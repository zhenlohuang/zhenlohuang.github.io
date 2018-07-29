---
layout: post
title: 'ThunderBird配置'
date: 2009-8-4
categories: [Others]
tags: [ThunderBird]
keywords: "ThunderBird"
description: 
comments: true
---

**thunderbird邮件提醒设置**
如果你使用 Ubuntu 9.04，给 ThunderBird 安装Mozilla Notification Extensions就可以让它使用 LibNotify 新通知系统
下载地址：[https://addons.mozilla.org/en-US/thunderbird/addon/11530](https://addons.mozilla.org/en-US/thunderbird/addon/11530)


**thunderbird最小化到托盘**
由于 thunderbird 没有最小化到托盘的扩展,这里用Alltray来模拟:
sudo apt-get install alltray

新建文件，thunderbirdStart.sh
内容如下:

``` bash
#!/bin/bash
alltray thunderbird
```
保存退出,并使文件可执行

打开System->Preferences->Sessions
添加此文件使之开机时运行

同样的方法也可以添加其他程序


添加联系人侧边栏
安装Contacts Sidebar插件就可以了
下载地址：[https://addons.mozilla.org/en-US/thunderbird/addon/70](https://addons.mozilla.org/en-US/thunderbird/addon/70)
