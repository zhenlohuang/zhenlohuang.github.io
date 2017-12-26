---
layout: post
title: '将Excel导入SQL Server'
date: 2010-9-29
wordpress_id: 472
permalink: /archives/the_excel_into_sql_server.html
categories: [Database]
tags: [Excel, SQL Server]
keywords: "Excel, SQL Server"
description: 
comments: true
---


将Excel导入SQL Server
1）外围应用配置器的设置。
 从"功能外围应用配置器"中选择"启动 OPENROWSET 和 OPENDATASOURCE 支持"选项。
2）
接受数据导入的表已经存在。
 

``` sql
  insert into t1 select * from OPENROWSET('MICROSOFT.JET.OLEDB.4.0' ,
  'Excel 5.0;HDR=YES;DATABASE=c://test.xls',sheet1$); 
```

导入数据并生成表。 

``` sql
  select * into t1 from OPENROWSET('MICROSOFT.JET.OLEDB.4.0',
  'Excel 5.0;HDR=YES;DATABASE=c://test.xls',sheet1$);
```

导入Excel中指定的列到数据库表中指定的列。

``` sql
  INSERT INTO t1(a1,a2,a3) SELECT a1,a2,a3 FROM OPENROWSET 'MICROSOFT.JET.OLEDB.4.0' ,'Excel5.0; HDR=YES; DATABASE=c://test.xls',sheet1$);
```

PS：导入时记得关闭Excel，默认情况下Excel的首行作为表头


