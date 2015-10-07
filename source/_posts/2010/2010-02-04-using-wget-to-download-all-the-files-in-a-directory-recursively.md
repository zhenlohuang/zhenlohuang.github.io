---
layout: post
title: '使用wget递归下载某目录下的所有文件'
date: 2010-2-4
wordpress_id: 391
permalink: /archives/using-wget-to-download-all-the-files-in-a-directory-recursively.html
categories: [Linux]
tags: [wget]
keywords: "wget"
description: 
comments: true
---
wget -c -r -nd -np -k -L -p -A c,h www.xxx.org/path/

-c 断点续传

-r 递归下载，下载指定网页某一目录下（包括子目录）的所有文件

-nd 递归下载时不创建一层一层的目录，把所有的文件下载到当前目录

-np 递归下载时不搜索上层目录。如wget -c -r www.xxx.org/pub/path/ 没有加参数

-np，就会同时下载path的上一级目录pub下的其它文件

-k 将绝对链接转为相对链接，下载整个站点后脱机浏览网页，最好加上这个参数

-L 递归时不进入其它主机，如wget -c -r www.xxx.org/ 如果网站内有一个这样的链接： www.yyy.org，不加参数 -L，就会像大火烧山一样，会递归下载www.yyy.org网站

-p 下载网页所需的所有文件，如图片等

-A 指定要下载的文件样式列表，多个样式用逗号分隔

-i 后面跟一个文件，文件内指明要下载的URL。
