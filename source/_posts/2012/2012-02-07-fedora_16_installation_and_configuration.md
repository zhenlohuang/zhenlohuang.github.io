---
layout: post
title: 'Fedora 16 安装后配置'
date: 2012-2-7
wordpress_id: 534
permalink: /archives/fedora_16_installation_and_configuration.html
categories: [Linux]
tags: [Fedora]
keywords: "Fedora"
description: 
comments: true
---

    
安装自动选择最快镜像插件    
 安装插件fastestmirror，可以让yum管理器自动搜索最快源下载

```
 sudo yum -y install yum-fastestmirror
```

添加rpmfusion源

```
 sudo yum rpm -ivh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm
```

安装GNOME-tweak-tool

```
 sudo yum install gnome-tweak-tool
```

标题栏添加“最大化/最小化/关闭”按钮    
 可以通过安装gnome-tweak-tool来设置。打开gnome-tweak-tool，“shell"-> Arrangement of buttons on the titlebar”可选择"All"

安装gconf-editor：

```
 sudo yum install gconf-editor
```

让桌面显示文件，激活右键功能    
 打开gnome-tweak-tool,"Desktop","Have file manager handle the desktop","开启"

安装鼠标右键“在终端中打开”

```
 sudo yum install nautilus-open-terminal
```

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

安装MSN客户端

```
 sudo yum install emesene
```

安装邮件提醒

```
 sudo yum install mail-notification mail-notification-evolution-plugin
```

安装FTP客户端

```
 sudo yum install filezilla
```

安装smplayer

```
 sudo rpm -ivh http://rpm.livna.org/livna-release.rpm
 sudo rpm -Uvh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm
 sudo rpm -Uvh http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-stable.noarch.rpm
 sudo yum -y install smplayer
```

安装Rhythmbox mp3 wma支持插件

```
 sudo yum install gstreamer-plugins-ugly gstreamer-plugins-bad gstreamer-ffmpeg
```

安装EasyTag

```
 sudo yum install easytag
```

安装g++

```
 sudo yum install gcc-c++
```
