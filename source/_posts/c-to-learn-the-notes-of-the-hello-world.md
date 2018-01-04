---
layout: post
title: 'C# 学习笔记之  Hello World'
date: 2010-7-7
wordpress_id: 446
categories: [Programming, C#]
tags: [C#]
keywords: "C#"
description: 
comments: true
---

晚上无聊看了点C#，随便写了个Hello World 做纪念

``` 
using System;
namespace MyNamespace
{
	class MyClass
	{
		static void Main()
		{
			Console.WriteLine("Hello World");
			Console.ReadLine();
			return ;
		}
	}
} 
```

保存为helloworld.cs,然后再cmd里面执行

```
csc helloworld.cs 
```

会在源文件的目录下面生成一个helloworld.exe的可执行程序
