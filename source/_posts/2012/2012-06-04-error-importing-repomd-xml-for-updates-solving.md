---
layout: post
title: 'Error importing repomd.xml for updates: Damaged repomd.xml file 解决方法'
date: 2012-6-4
wordpress_id: 2449
permalink: /archives/error-importing-repomd-xml-for-updates-solving.html
categories: [Linux]
tags: [Fedora]
keywords: "Fedora"
description: 
comments: true
---
yum执行时出现错误：Error importing repomd.xml for updates: Damaged repomd.xml file

解决方法：
重新下载<http://mirrors.sohu.com/fedora/releases/16/Everything/i386/os/repodata/repomd.xml>,覆盖到/var/cache/yum/i386/16/fedora/
