---
layout: post
title: 'Cannot retrieve metalink for repository: fedora. Please verify its path and try again 解决方法'
date: 2012-6-4
wordpress_id: 2451
categories: [Linux]
tags: [Fedora]
keywords: "Fedora"
description: 
comments: true
---
执行如下命令：

``` bash
su -c "sed -i 's|^#baseurl|baseurl| ; s|^mirrorlist|#mirrorlist|' /etc/yum.repos.d/*"
```

参考资料：<http://ekd123.is-programmer.com/posts/25018.html>
