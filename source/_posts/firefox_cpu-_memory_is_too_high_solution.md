---
layout: post
title: 'Firefox占用CPU、内存太高解决办法'
date: 2009-10-6
wordpress_id: 348
categories: [Others]
tags: [Firefox]
keywords: "Firefox"
description: 
comments: true
---

CPU解决：
首先解决CPU占用率高，打开网页停顿的问题。
很简单，在"工具"/"选项"/"内容"里，找到"启用Java"这一项，去掉前面的勾，然后确认，重启即可解决问题


内存解决：
为Firefox设置快速缓存

　　1、打开Firefox浏览器。

　　2、在地址栏中输入"about:config"。

　　3、在"过滤器"中输入"browser.cache.memory.enable"。将其改为true

　　4、在浏览器中右键点击后选择"新建">"整数"。输入"browser.cache.memory.capacity"后点击"确定"。接着，你需要在此输入一个值，而这个值的大小则取决于你计算机物理内存的大小。内存数*16即可
　　 PS：如果你想要恢复默认设置的话，你可以将"browser.cache.memory.capacity"的值改为"-1"。

    5、重新启动Firefox即可。

 当Firefox最小化时释放内存设置
 
　　1、打开Firefox浏览器。

　　2、在地址栏中输入"about:config"。

　　3、在浏览器中右键点击，选择"新建">"布尔"。

　　4、在弹出的窗口中输入"config.trim_on_minimize"，接着点击"确定"。选中"true"，接着点击"确定"。

    5、重新启动Firefox即可。
