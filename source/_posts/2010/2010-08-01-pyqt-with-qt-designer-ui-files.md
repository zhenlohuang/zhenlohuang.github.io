---
layout: post
title: 'PyQt 使用Qt Designer ui文件'
date: 2010-8-1
wordpress_id: 449
permalink: /archives/pyqt-with-qt-designer-ui-files.html
categories: [Programming]
tags: [Python, Qt, PyQt]
keywords: "Python, Qt, PyQt"
description: 
comments: true
---
首先用Qt Designer 创建窗体后，保存为form.ui

然后再cmd中输入

``` python 
pyuic4 -o ui_form.py form.ui 
```

之后对应目录下生成ui_form.py的文件

附上pyuic4的帮助

``` python 
NAME
       pyuic4 - compile Qt4 user interfaces to Python code
SYNOPSIS
       pyuic4 [OPTION]... FILE
DESCRIPTION
       pyuic4  takes  a Qt4 user interface description file and compiles it to
       Python code. It can also show a preview of the user interface.
OPTIONS
       -h, --help
              Show a summary of the options.
       --version
              Display the version number of pyuic4 of the version of Qt  which
              PyQt4 was generated for.
       -p, --preview
              Show a preview of the UI instead of generating Python code.
       -o, --output=FILE
              Write the generated Python code to FILE instead of stdout.
       -d, --debug
              Show  detailed  debugging  information  about  the UI generation
              process.
       -x, --execute
              Generate extra code to test and display the class when  executed
              as a script.
       -i, --indent=NUM
              Set the indentation width to NUM spaces. A TAB character will be
              used if NUM is 0 (default: 4). 
```
