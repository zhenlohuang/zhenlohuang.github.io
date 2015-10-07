---
layout: post
title: 'Java安装及配置'
date: 2009-5-22
wordpress_id: 294
permalink: /archives/java-installation-and-configuration.html
categories: [Programming]
tags: [Java]
keywords: "Java"
description: 
comments: true
---
系统：Ubuntu 9.04

Java版本：java 6

*安装jre*

```
sudo apt-get install sun-java6-jre
```
如果空间富裕，建议安装一个JDK。

```
sudo apt-get install sun-java6-jdk
```
提示：安装过程中需要你回答是否同意使用协议（终端中红蓝色的提示界面），此时按tab键至OK，再按回车即可正常安装。

设置当前默认的java解释器：

```
sudo update-alternatives --config java
```
配置JAVA环境变量:

```
sudo gedit /etc/environment
```
在其中添加如下两行：

```
CLASSPATH=.:/usr/lib/jvm/java-6-sun/lib
JAVA_HOME=/usr/lib/jvm/java-6-sun
```
*Java中文支持*

进入java字体目录创建一个新目录

```
cd /usr/lib/jvm/java-6-sun/jre/lib/fonts
sudo mkdir fallback
```
复制字体

```
sudo cp /usr/share/fonts/truetype/arphic/* /usr/lib/jvm/java-6-sun/jre/lib/fonts/fallback
```