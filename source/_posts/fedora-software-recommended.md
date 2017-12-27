---
layout: post
title: 'Fedora 常用软件推荐'
date: 2012-6-4
wordpress_id: 2453
categories: [Linux]
tags: [Fedora]
keywords: "Fedora"
description: 
comments: true
---
# 系统工具
1）安装自动选择最快镜像插件
安装插件fastestmirror，可以让yum管理器自动搜索最快源下载

```
sudo yum -y install yum-fastestmirror
```

2）添加rpmfusion源

```
sudo yum rpm -ivh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm
```

3）安装GNOME-tweak-tool

```
sudo yum install gnome-tweak-tool
```

4）标题栏添加“最大化/最小化/关闭”按钮
可以通过安装gnome-tweak-tool来设置。打开gnome-tweak-tool，“shell"-> Arrangement of buttons on the titlebar”可选择"All"

5)让桌面显示文件，激活右键功能
打开gnome-tweak-tool,"Desktop","Have file manager handle the desktop","开启"

6)安装gconf-editor：

```
sudo yum install gconf-editor
```

7)安装鼠标右键“在终端中打开”

```
sudo yum install nautilus-open-terminal
```

---

# 常用工具
安装Gnome Do

```
sudo yum install gnome-do
```

安装Compiz

```
sudo yum install compiz-manager
sudo yum install ccsm
```

安装Docky

```
sudo yum install docky
```

安装解压缩软件7z

```
sudo yum install p7zip p7zip-plugins
```

安装截图工具shutter

```
sudo yum install shutter
```

安装小熊猫

```
sudo yum install ailurus
```

---

# 多媒体
安装amarok

```
sudo yum install amarok
```

安装smplayer

```
sudo yum install smplayer
```

安装EasyTag

```
sudo yum install easytag
```

---

# 网络应用
安装Chrome
URL: www.google.cn/Chrome

安装FTP客户端

```
sudo yum install filezilla
```

---

# 通讯工具
安装MSN客户端

```
sudo yum install emesene
```

---

#开发工具
gcc

```
sudo yum install gcc
```

g++

```
sudo yum install gcc-c++
```

autoconf && automake

```
sudo yum install autoconf automake
```

gdb

```
sudo yum install gdb
```

Geany

```
sudo yum install geany
```

vim

```
sudo yum install vim
```

Subversion

```
sudo yum install subversion
```

Git

```
sudo yum install git
```

Eclipse

```
sudo yum install eclipse-platform
```

CDT

```
sudo yum install eclipse-cdt
or http://download.eclipse.org/tools/cdt/releases/indigo
```

PyDev

```
sudo yum install eclipse-pydev
or http://pydev.org/updates
```

Subclipse

```
sudo yum install eclipse-subclipse
or http://subclipse.tigris.org/update_1.8.x
```

Texlipse

```
sudo yum install eclipse-texlipse
or http://texlipse.sourceforge.net
```

