---
layout: post
title: 'windows下netbeans 6.5 C/C++的配置'
date: 2009-2-26
wordpress_id: 263
categories: [Programming]
tags: [Netbeans]
keywords: "Netbeans"
description: 
comments: true
---
（1）首先下载最新版本的netbeans6.5,下载后选择全部安装，还要下载一个C++编译器.我们选择Cygwin.

（2）Cygwin配置如下
1.运行 setup.exe 程序。接受缺省设置，直至转入 "Select Your Internet Connection" 页。在此页上选择最适合您的选项。单击“下一步”。

2.在 "Choose A Download Site" 页上，选择一个方便您下载的站点。单击“下一步”。

3.在 "Select Packages" 页上，选择要下载的包。单击 "Devel" 旁边的 "+" 号，以展开此开发工具类别。您可能   需要调整窗口的大小，以便一次可以看到更多的内容。 通过单击包旁边的 "Skip" 标签来选择要下载的每个包。您至少要选择

gcc-core: C compiler

gcc-g++: C++ compiler

gdb: The GNU Debugger

make: the GNU version of the 'make' utility。
然后安装.

（3）现在将编译器目录添加到您的 Path 变量中： 将 cygwin-directory/usr/bin 和 cygwin-directory/bin 目录的路径添加到 Path 变量中，然后单击“确定”。缺省情况下，cygwin-directory 为 C:/cygwin。

（4）启动netbeans就可以了.你可以选择 Tools -&gt; options -&gt; C/C++ 下面的选项框中是不是自动就填好了. 如果以上运行正常,你就可以用netbeans开发你的C/C++ Application了.
