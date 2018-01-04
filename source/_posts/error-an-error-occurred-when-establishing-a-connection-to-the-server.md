---
layout: post
title: '错误：在建立与服务器的连接时出错。在连接到 SQL Server 2005 时，在默认的设置下 SQL Server 不允许进行远程连接可能会导致此失败。 (provider: SQL 网络接口, error: 26 - 定位指定的服务器/实例时出错) 解'
date: 2010-9-29
wordpress_id: 474
categories: [Database, SQL Server]
tags: [SQL Server]
keywords: "SQL Server"
description: 
comments: true
---
在建立与服务器的连接时出错。在连接到 SQL Server 2005 时，在默认的设置下 SQL Server 不允许进行远程连接可能会导致此失败。 (provider: SQL 网络接口, error: 26 - 定位指定的服务器/实例时出错)

解决方法：开始-> 所有程序-> Ms   Sql   Server-> 配置工具-> sql   server外围应用配置器-> 服务和连接的外围应用配置器-> 打开MSSQLSERVER节点下的Database   Engine   节点,先择 "远程连接 ",接下建议选择 "同时使用TCP/IP和named   pipes ",确定后,重启数据库服务就可以了.

如何还是不行就是connectionString出问题了
