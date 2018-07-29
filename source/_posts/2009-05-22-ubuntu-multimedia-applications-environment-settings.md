---
layout: post
title: 'Ubuntu 多媒体应用环境设置'
date: 2009-5-22
categories: [Linux]
tags: [Ubuntu]
keywords: "Ubuntu"
description: 
comments: true
---
*加入Medibuntu源*

```
sudo wget  http://www.medibuntu.org/sources.list.d/jaunty.list  -O /etc/apt/sources.list.d/medibuntu.list
sudo apt-get update &amp;&amp; sudo apt-get install medibuntu-keyring &amp;&amp; sudo apt-get update
```
*安装Flash 插件*

```
sudo apt-get install adobe-flashplugin
```
*安装多媒体解码器*

```
* Xine多媒体引擎解码器
```
sudo apt-get install libxine1-ffmpeg libxine1-all-plugins libxine1-plugins w32codecs libstdc++5

```
* Gstreamer多媒体引擎解码器
```
sudo apt-get install gstreamer0.10-ffmpeg gstreamer0.10-pitfdll gstreamer0.10-plugins-bad gstreamer0.10-plugins-bad-multiverse gstreamer0.10-plugins-ugly gstreamer0.10-plugins-ugly-multiverse gstreamer0.10-esd

```
* DVD影碟功能支持
```
sudo apt-get install libdvdnav4 libdvdread4
sudo /usr/share/doc/libdvdread4/install-css.sh

```
