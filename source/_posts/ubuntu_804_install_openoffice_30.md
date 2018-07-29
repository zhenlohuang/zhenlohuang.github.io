---
layout: post
title: 'Ubuntu 8.04安装OpenOffice 3.0'
date: 2008-11-22
categories: [Linux]
tags: [Ubuntu]
keywords: "Ubuntu"
description: 
comments: true
---

先去OpenOffice网站下载一个OOo_3.0.0_LinuxIntel_install_zh-cn_deb.tar.gz
1. 进入DEBS文件夹，执行  sudo dpkg -i *.deb

2. sudo apt-get autoremove openoffice.org-common*

3. 进入DEBS里面的desktop-integration文件夹安装执行

``` bash
sudo dpkg -i openoffice.org3.0-debian-menus_3.0-9354_all.deb
```

当然可以添加源：

``` bash
deb http://ppa.launchpad.net/openoffice-pkgs/ubuntu intrepid main
```