---
layout: post
title: 'Ant 设置代理'
date: 2014-6-12
wordpress_id: 4916
categories: [Programming]
tags: [Java, Ant]
keywords: "Java, Ant"
description: 
comments: true
---
最简单的方式就是在build.xml里面加入下面代码：

``` xml
<setproxy proxyhost="proxy_host" proxyport="proxy_port"/>
```
更多细节可以参考：    
Proxy Configuration：<https://ant.apache.org/manual/proxy.html>    
Setproxy Task：<https://ant.apache.org/manual/Tasks/setproxy.html>
