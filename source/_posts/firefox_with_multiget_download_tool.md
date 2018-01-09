---
layout: post
title: 'Firefox搭配Multiget下载工具'
date: 2009-8-4
categories: [Others]
tags: [Firefox]
keywords: "Firefox"
description: 
comments: true
---

Multiget安装

```
 sudo apt-get install multiget
```

把Multiget和Firefox连接起来的方法很简单，通过Flashgot插件完成
1. 打开Flashgot选项，点击"常规"标签页。

2. 因为下载管理器里面是没有Multiget的，所以点击"新增"，填入Multiget。

3. 选择程序/usr/bin/multiget

4. 在参数模板中写入，把全角的括号改成半角的。
引用

```［url=URL］［refer=REFERER］```

5. 确定OK。
