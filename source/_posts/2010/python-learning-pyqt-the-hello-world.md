---
layout: post
title: 'Python 学习  PyQt Hello World'
date: 2010-8-5
wordpress_id: 462
categories: [Programming]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
Qt 开发库是一个使用广泛的跨平台 GUI 开发库，可用于 Windows、Linux、Mac OSX 和许多手持平台。QT 具有良好结构化（但灵活）的面向对象的结构、清晰的文档以及直观的 API。自Trolltech公司被Nokia收购后，Qt成为Nokia旗下的一个部门。

Python的默认GUI是Tkinter，PyQt是跨平台应用程式框架 Qt 的 Python绑定版本，同时也是PyKDE（KDE API 的Python绑定）的基础。PyQt支持Linux操作系统和其他Unix ，以及Mac OS X操作系统和微软Windows 。

``` python 
#!/usr/bin/env python
#PyQt Hello World
import sys
from PyQt4 import QtCore,QtGui
app = QtGui.QApplication(sys.argv)
label = QtGui.QLabel("Hello World", None)
label.show()
sys.exit(app.exec_()) 
```
