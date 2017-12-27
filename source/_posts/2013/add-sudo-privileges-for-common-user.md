---
layout: post
title: 'Linux为用户添加sudo权限'
date: 2013-10-19
wordpress_id: 4148
categories: [Linux]
tags: []
keywords: ""
description: 
comments: true
---
用sudo时提示"xxx is not in the sudoers file. This incident will be reported.其中XXX是你的用户名，也就是你的用户名没有权限使用sudo。

解决方法：修改/etc/sudoers。步骤如下：    
1) 使用root用户

``` bash
su -
```

2) 添加文件写权限

``` bash
chmod u+w /etc/sudoers
```

3) 编辑/etc/sudoers

vi /etc/sudoers    
在root ALL=(ALL) ALL添加killua ALL=(ALL) ALL

4) 撤销写权限

``` bash
chmod u-w /etc/sudoers
```


