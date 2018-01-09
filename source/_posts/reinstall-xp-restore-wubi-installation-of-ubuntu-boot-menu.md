---
layout: post
title: '重装xp后恢复wubi安装的Ubuntu启动菜单'
date: 2009-7-7
categories: [Linux]
tags: [Ubuntu]
keywords: "Ubuntu"
description: 
comments: true
---
1，把ubuntu->winboot文件夹下wubidr和wubidr.mbr两个文件拷到C盘根目录下

2，打开boot.ini文件，添加c:/wubildr.mbf= "Ubuntu",保存修改

PS:boot.ini在C盘的根目录下，要是看不到，进入文件夹选项，<span lang="EN-US">去掉“隐藏受保护的操作系统文件”选项，选中"显示所有文件和文件夹",就可以看到boot.ini了
