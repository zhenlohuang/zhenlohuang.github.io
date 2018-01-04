---
layout: post
title: '错误：[IM002] [Microsoft][ODBC 驱动程序管理器] 未发现数据源名称并且未指定默认驱动程序 解决方法'
date: 2010-9-29
wordpress_id: 473
categories: [Database, SQL Server]
tags: [ODBC]
keywords: "ODBC"
description: 
comments: true
---


错误：[IM002] [Microsoft][ODBC 驱动程序管理器] 未发现数据源名称并且未指定默认驱动程序

解决方法：
在管理工具里面->点数据源ODBC-> 系统DSN->添加 选SQL 然后找到你要连接的数据库
这里的系统DSN 的配置要跟 用户DSN里的配置一样。


