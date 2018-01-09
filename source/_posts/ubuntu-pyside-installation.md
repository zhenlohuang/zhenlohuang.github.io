---
layout: post
title: 'Ubuntu上PySide安装'
date: 2013-4-14
categories: [Programming, Python]
tags: [Python, Qt, PySide]
keywords: "Python, Qt, PySide"
description: 
comments: true
---
PySide 是跨平台的应用程式框架 Qt 的 Python 绑定版本 。在2009年8月，PySide首次发布。提供和 PyQt 类似的功能，并相容 API。但与 PyQt 不同处为使用LGPL授权。

安装命令：

``` bash
sudo add-apt-repository ppa:pyside
sudo apt-get update
sudo apt-get install python-pyside
```
如果想只装某个模块：

``` bash
sudo apt-get install python-pyside.qtgui
```
