---
layout: post
title: '【资料整理】MEF'
date: 2010-9-5
wordpress_id: 467
categories: [Programming]
tags: [C#]
keywords: "C#"
description: 
comments: true
---
**什么是MEF？**

　Managed Extensibility Framework（MEF）可以很容易的构造可扩展性的应用程序。MEF提供了发现和组合能力，因此你可以选择来加载插件。

**MEF解决了什么问题？**

- MEF赠送了一种简单的在运行时扩展问题。直到现在，任何程序你想支持插件模式，需要构建自己的架构。这些插件经常是特定应用的并且不能被多种实现重用的。
- MEF提供一个标准方式来让程序暴露自己，消耗外部扩展。扩展，天生的，能被不同的程序重用。然而，一个扩展需要实现特定应用。扩展可以依赖另一个扩展，MEF确保它们以正确的顺序同时被装配。
- MEF允许在查询和筛选中使用额外的元数据来标记扩展。

**MEF如何工作？**

- 粗略的说，MEF的核心由一个catalog（目录）和一个CompositionContainer组成。一个catalog负责查找扩展，container负责协调创建和符合的依赖。
- MEF第一个类是ComposablePart。一个Composable part提供了一个或多个Exports，也可以依赖于一个或多个由Imports提供的扩展。一个composable part也管理可以是object或给定类型的实例。MEF，然而，是一个可扩展的，额外的ComposablePart实现，它依附于Import/Export契约。
- Exports和Imports每个都有一个契约(Contract)。契约(Contract)是exports和Imports之间的桥梁。一个export契约由用来过滤的元数据组成。例如，它可以指定一个export提供的指定能力。
- MEF的容器和Catalogs交互来访问composable parts。容器自己解决一部分依赖并暴露Exports给外界。如果你愿意，你可以自由去增加composable part的实例。
- 一个ComposablePart返回一个catalog，它在你的程序中很像一个扩展。它有宿主程序提供的Imports，然后Export其他。
- 默认MEF composable part实现使用attribute元数据来声明exports和imports。这允许MEF来决定哪个parts, imports和exports是完全可用的。

![image](/images/uploads/2010/09/FileDownload.aspx?ProjectName=MEF&DownloadId=50697)
