---
layout: post
title: 'Ubuntu下django安装'
date: 2011-4-14
wordpress_id: 504
categories: [Programming]
tags: [Python, Django]
keywords: "Python, Django"
description: 
comments: true
---
系统版本：Ubuntu 10.10

*Python安装*

``` python 
sudo apt-get install python
```
*Apache安装*

``` python 
sudo apt-get install apache2
```
*mod_python模块安装*

``` python 
sudo apt-get install libapache2-mod-python
```
*测试Apache*

重启电脑后，在浏览器中输入：http://localhost

出现"It works"说明安装成功

*安装django*

下载Django-1.3.tar.gz

``` python 
tar xzvf Django-1.3.tar.gz
cd Django-1.3
sudo python setup.py install
```
*测试django*

在Python命令行中输入

``` python 
 import django
    print django.get_version()
```
如果无错误则安装成功。

*mysql安装*

``` python 
sudo apt-get install mysql-server mysql-client mysql-admin
```
python mysql支持：

``` python 
sudo  apt-get install python-mysqldb
```
