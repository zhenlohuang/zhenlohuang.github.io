---
layout: post
title: 'Javascript 删除字符串空格'
date: 2012-5-23
wordpress_id: 2434
permalink: /archives/javascript-delete-string-space.html
categories: [Web]
tags: [javascript]
keywords: "javascript"
description: 
comments: true
---

1）删除左右两端的空格

``` js
function trim(str){
return str.replace(/(^\s*)|(\s*$)/g, "");
```

2) 删除左边的空格

``` js
function ltrim(str){
return str.replace(/(^\s*)/g,"");
}
```

3) 删除右边的空格

``` js
function rtrim(str){
return str.replace(/(\s*$)/g,"");
}
```
