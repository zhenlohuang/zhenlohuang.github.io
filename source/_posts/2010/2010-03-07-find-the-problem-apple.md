---
layout: post
title: '【IT笔试面试题整理】查找问题苹果'
date: 2010-3-7
wordpress_id: 404
permalink: /archives/find-the-problem-apple.html
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---


**【试题描述】**
问题：10个苹果，有一个苹果有问题，可能轻可能重，用三次找到问题苹果

**【试题来源】**未知

**【试题分析】**
先分组3 3 4,设第一组为A，第二组为B，第三组为C，然后拿3和3放天枰上
if (A ==B) {
问题苹果在C组里面
然后C组拿两个C1，C2放到各放一个到A，B组中
 if(天枰偏移) {
 证明问题苹果在放入的苹果C1，C2中，任取苹果C3换下C1。
 If（天枰平衡）{
 问题苹果为C1
}else {
 问题苹果为C2
}
} else {
 证明问题苹果在剩下的苹果C3，C4中，任取苹果C3换下C1。
 If（天枰平衡）{
 问题苹果为C4
}else {
 问题苹果为C3
}
}
} else {
证明问题苹果在A组或B组里面
 然后取下A组，从C组中拿出3个换上
 If（天枰平衡）{
 问题苹果在A组中，取下所有苹果从A组中拿两个放上去，问题解决。
}else {
问题苹果在B组中，取下所有苹果从B组中拿两个放上去，问题解决。
}
}

