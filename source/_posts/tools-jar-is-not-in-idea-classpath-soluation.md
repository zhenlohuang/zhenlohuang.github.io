---
layout: post
title: 'tools.jar is not in IDEA classpath 解决方法'
date: 2013-11-24
wordpress_id: 4837
categories: [Programming, Java]
tags: [JAVA, IDEA]
keywords: "JAVA, IDEA"
description: 
comments: true
---
在Ubuntu中，启动IntelliJ IDEA时出错，错误如下：

![image](/images/uploads/2013/11/Screenshot-from-2013-11-24-203357.png)

解决方法：

修改idea.sh文件，添加一行在开头

``` bash 
JAVA_HOME=/home/killua/Dev/jdk1.7.0_45
```
