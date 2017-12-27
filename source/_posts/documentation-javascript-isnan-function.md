---
layout: post
title: '【资料整理】Javascript中isNaN 函数'
date: 2010-12-13
wordpress_id: 490
categories: [Web]
tags: [Javascript]
keywords: "Javascript"
description: 
comments: true
---
isNaN 函数

``` js
isNaN(expression:Object) : Boolean
```

计算参数，如果值为 NaN（非数字），则返回 true。此函数可用于检查一个数学表达式是否成功地计算为一个数字。

**参数**   
expression:Object - 要计算的布尔值、变量或其它表达式。

**返回**   
Boolean - 一个布尔值。

**例子：**   

``` js
if(isNaN(document.login.imgcode.value)){
   alert('验证码必须是数字！')
   document.login.imgcode.focus();
   return false;
}
```
