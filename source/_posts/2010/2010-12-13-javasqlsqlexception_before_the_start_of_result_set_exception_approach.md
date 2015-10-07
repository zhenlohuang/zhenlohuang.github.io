---
layout: post
title: 'java.sql.SQLException: Before start of result set异常及处理办法'
date: 2010-12-13
wordpress_id: 487
permalink: /archives/javasqlsqlexception_before_the_start_of_result_set_exception_approach.html
categories: [Web]
tags: [Javascript]
keywords: "Javascript"
description: 
comments: true
---


**异常：**java.sql.SQLException: Before start of result set

**解决方法：**使用rs.getString();前一定要加上rs.next();

**原因：**ResultSet对象代表SQL语句执行的结果集，维护指向其当前数据行的光标。每调用一次next()方法，光标向下移动一行。最初它位于第一行之前，因此第一次调用next()应把光标置于第一行上，使它成为当前行。随着每次调用next()将导致光标向下移动一行。在ResultSe对象及其t父辈Statement对象关闭之前，光标一直保持有效。

