---
layout: post
title: '解决Github网站CSS无法加载问题'
date: 2014-5-10
categories: [Others]
tags: [Github]
keywords: "Github"
description: 
comments: true
---
最近在Linux使用GoAgent+SwitchySharp下，Github无法加载CSS。

![image](/images/uploads/2014/05/Selection_001.png)

在Chrome中按下F12进行调试，重新加载页面显示如下：

![image](/images/uploads/2014/05/Selection_002.png)

由此可见，CSS无法加载的原因就在于，GoAgent无法连接到github.global.ssl.fastly.net。因此解决方法只要将github.global.ssl.fastly.net加入SwitchySharp的rule就可以了，使其Direct Connection。

![image](/images/uploads/2014/05/Selection_003.png)