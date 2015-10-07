---
layout: post
title: '在Linux中安装QQ2008'
date: 2008-11-1
wordpress_id: 232
permalink: /archives/linux_installation_qq2008.html
categories: [Linux]
tags: [Ubuntu]
keywords: "Ubuntu"
description: 
comments: true
---

第一步，安装wine，
sudo apt-get install wine

第三步，下载以下文件：mfc42.dll，msvcp60.dll，riched20.dll，riched32.dll 然后拷贝到 /home/killua/.wine/drive_c/windows/system32 里，覆盖原有文件。

第四步，安装QQ。
以wine的方式运行QQ的安装文件
wine QQ2008。exe

接下来，我们便可以看到在Windows下常见的QQ安装窗口了，安装过程跟Windows下完全一样，一步一步“下一步”就行了。在这里我要提醒一点，QQ主程序的安装路径最好选默认值，系统会自动将其存放到Linux虚拟的WindowsXP C盘的相应位置中，这样可防止过后执行过程中出现一些未知的错误。

第五步，安装结束以后，把QQ安装目录 /home/killua/./wine/drive_c/Program Files/Tencent/QQ 里的 TXPlatform.exe 删除掉。

第六步，为QQ设置一下wine。
在终端中输入下面的命令打开wine的配置文件
winecfg
在”Applications”标签里添加QQ的主执行程序QQ.exe；在”Windows Version”下拉框中选择”WindowsXP”；完成上述两步以后，点击“应用”，然后切换到”Libraries”标签，在”New override for library”下拉框中添加riched20和riched32，最后确定退出。

第七步，运行QQ。
cd /home/killua/./wine/drive_c/Program Files/Tencent/QQ
wine QQ.exe
