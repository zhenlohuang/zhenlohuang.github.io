---
layout: post
title: 'VS2008 下安装 AjaxControlToolkit'
date: 2011-1-24
wordpress_id: 493
categories: [Web Development]
tags: [ajax]
keywords: "ajax"
description: 
comments: true
---


1）下载 AjaxControlToolkit
 <http://ajaxcontroltoolkit.codeplex.com/releases/view/11121>
 建议下载AjaxControlToolkit-Framework3.5-NoSource.zip

2）将AjaxControlToolkit集成到VS2008中
 打开 VS2008 新建项目->ASP.NET 应用程序。
 右键单击工具栏，选择【添加选项卡】并命名为AJAX Control Toolkit，然

后在该选项卡内部右键选择【选择项】。
 选择AjaxControlToolkit-Framework3.5-NoSource/SampleWebSite/Bin/AjaxControlToolkit.dll ,这个时候应该工具箱

里面会生成相应的控件

3）有如下两种方法    

``` aspx-cs    
 <%@ Register Assembly= "AjaxControlToolkit" Namespace = "AjaxControlToolkit" TagPrefix = "ajax" %> 加到aspx里面。
````    
 或者
修改web.config，添加

``` aspx-cs    
<pages>
 <controls>
....
   <add tagPrefix="ajax" namespace="AjaxControlToolkit" assembly="AjaxControlToolkit , PublicKeyToken=31BF3856AD364E35"/> 
....  
 </controls>
</pages> 
```

PS：之前一直有问题是在集成完工具后，无法使用控件。个人认为可能是因为下载的那个AjaxControlToolkit有问题，后面下载了个AjaxControlToolkit-Framework3.5，问题解决。
