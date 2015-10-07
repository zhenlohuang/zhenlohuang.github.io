---
layout: post
title: 'Qt出错信息”Basic XLib functionality test failed!”解决'
date: 2009-11-19
wordpress_id: 364
permalink: /archives/qt-error-message-quotbasic-xlib-functionality-test-failedquot-to-solve.html
categories: [Programming]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---
缺少了libX11的开发库

解决方案：sudo apt-get install libX11-dev libXext-dev libXtst-dev
