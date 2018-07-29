---
layout: post
title: "Sublime Text的Markdown写作"
date: 2015-08-30
categories: [Others]
tags: [Markdown, SublimeText]
keywords: 
description: 
comments: true
---

Sublime Text是一个十分强大的文本编辑器，它支持Windows，Linux和Mac。Sublime Text在默认情况下就支持Markdown格式，本文想介绍的是如何增强Sublime Text使其更好的Markdown写作。

# Package Control

想必大多数SublimeText的用户已经安装了Package Control这个神器了。如未安装，请参考：https://packagecontrol.io/installation

# Markdown Preview

markdown preview这个插件可以使用户在浏览器中预览Markdown文件。

按下键Ctrl+Shift+p调出命令面板，找到Package Control: install Pakage这一项。搜索markdown preview，点击安装。

关于快捷键设置，可以在Preferences->Key Binding User中，加入
```
	{ 
		"keys": ["alt+m"],
		"command": "markdown_preview",
		"args": 
		{ 
			"target": "browser"
			
		} 
	}
```

# 语法高亮

默认SublimeText带的语法高亮对Markdown确实太少了，可以装一个Monokai Extended插件进行增强。在Package Control中搜索Monokai Extended并安装，然后在Preferences->Color Scheme中进行设置。

