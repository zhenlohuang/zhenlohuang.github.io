---
layout: post
title: 'Ubuntu LAMP环境搭建'
date: 2010-4-12
wordpress_id: 413
permalink: /archives/ubuntu-the-lamp-environment-to-build.html
categories: [Programming]
tags: [PHP]
keywords: "PHP"
description: 
comments: true
---
*安装Apache*

sudo apt-get install apache2

**安装PHP5**

sudo apt-get install php5

**安装MySQL**

sudo apt-get install mysql-server mysql-common mysql-admin

**安装PHP-MySQL驱动**

sudo apt-get install php5-mysql

在浏览器中输入：http://localhost查看

站点的程序默认在：/var/www/
apache配置文件在：/etc/apache2
php配置文件在：/etc/php5/
