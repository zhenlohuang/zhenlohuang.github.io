---
layout: post
title: 'Qt Embedded 开发环境搭建'
date: 2009-11-6
wordpress_id: 363
permalink: /archives/qt_embedded_development_environment_to_build.html
categories: [Programming]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---

具体步骤与说明:

　　1. 下载源码包

　　qt-x11-opensource-src-4.5.3.tar.bz2

　　qt-embedded-linux-opensource-src-4.5.3.tar.bz2


　　2.编译及安装qt-x11-opensource-src-4.5.3

　　tar xjvf qt-x11-opensource-src-4.5.3.tar.bz2

　　cd qt-x11-opensource-src-4.5.3

　　./configure

　　make

　　sudo make install //这个要管理员权限，因为涉及到系统文件操作

　　默认安装在/usr/local/Trolltech/Qt-4.5.0下

　　3.编译及安装qt-embedded-linux-opensource-src-4.5.3

　　tar xjvf qt-embedded-linux-opensource-src-4.5.3.tar.bz2

 mv qt-embedded-linux-opensource-src-4.5.3 qt-embedded-linux-opensource-src-4.5.3-arm

　　cd qt-embedded-linux-opensource-src-4.5.3-arm

　　./configure -embedded arm -qvfb

　　make

　　sudo make install

　　4.设置环境变量

　　(1)qt-x11:

　　vi setenv-x11.sh

　　添加如下内容:

　　PATH=/usr/local/Trolltech/Qt-4.5.3/bin:$PATH

　　LD_LIBRARY_PATH=/usr/local/Trolltech/Qt-4.5.3/lib:$LD_LIBRARY_PATH

　　保存退出.移到/usr/local/Trolltech/Qt-4.5.3中。


　　(2)qt-embedded-arm:

　　vi setenv-arm.sh

　　添加如下内容:

　　QTEDIR=/usr/local/Trolltech/QtEmbedded-4.5.3-arm

　　PATH=/usr/local/Trolltech/QtEmbedded-4.5.3-arm/bin:$PATH

　　LD_LIBRARY_PATH=/usr/local/Trolltech/QtEmbedded-4.5.3-arm/lib:$LD_LIBRARY_PATH

　　保存退出.移到/usr/local/Trolltech/QtEmbedded-4.5.3-arm中。


　　5.编译qvfb

　　cd qt-x11-opensource-src-4.5.3

　　cd /tools/qvfb

　　make 
 
 sudo make install

　　6.测试

　　cd /usr/local/Trolltech/QtEmbedded-4.5.3-arm/demos/books

　　qvfb -width 640 -height 480 &

　　./books -qws
