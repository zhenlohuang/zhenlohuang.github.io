---
layout: post
title: '【资料整理】JavaScript中getElementsByName()和getElementById()的区别和用法'
date: 2010-12-13
wordpress_id: 486
categories: [Web Development]
tags: [JavaScript]
keywords: "JavaScript"
description: 
comments: true
---
**getElementsByName()定义和用法**
getElementsByName() 方法可返回带有指定名称的对象的集合。
语法    

``` js
document.getElementsByName(name)
```
该方法与 getElementById() 方法相似，但是它查询元素的 name 属性，而不是 id 属性。
另外，因为一个文档中的 name 属性可能不唯一（如 HTML 表单中的单选按钮通常具有相同的 name 属性），所有 getElementsByName() 方法返回的是元素的数组，而不是一个元素。

**getElementById()定义和用法**
getElementById() 方法可返回对拥有指定 ID 的第一个对象的引用。
语法    

``` js
document.getElementById(id) 
```
说明   
HTML DOM 定义了多种查找元素的方法，除了 getElementById() 之外，还有 getElementsByName() 和 getElementsByTagName()。
不过，如果您需要查找文档中的一个特定的元素，最有效的方法是 getElementById()。
在操作文档的一个特定的元素时，最好给该元素一个 id 属性，为它指定一个（在文档中）唯一的名称，然后就可以用该 ID 查找想要的元素。

PS：要注意getElementsByName()返回的是一个集合，即使只有一个，也要加上下标索引

