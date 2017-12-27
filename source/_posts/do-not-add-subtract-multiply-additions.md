---
layout: post
title: '【IT笔试面试题整理】不用加减乘除做加法'
date: 2012-10-6
wordpress_id: 3441
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】写一个函数，求两个整数的和，要求在函数体内不得使用加减乘除四则运算符合。

【试题来源】未知

【参考代码】

``` cpp
int add(int num1, int num2) {

	int sum;
	int carry;
	do {
		sum = num1 ^ num2;
		carry = (num1 & num2) << 1;

		num1 = sum;
		num2 = carry;
	} while(num2 != 0);

	return num1;
}
```
