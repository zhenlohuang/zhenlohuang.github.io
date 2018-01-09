---
layout: post
title: 'Qt出错信息”Basic XLib functionality test failed!”解决'
date: 2009-11-19
categories: [Programming, C++]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---
缺少了libX11的开发库

解决方案：sudo apt-get install libX11-dev libXext-dev libXtst-dev
