---
layout: post
title: '【IT笔试面试题整理】数组中出现次数超过一半的数字'
date: 2012-10-6
wordpress_id: 3426
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

【试题来源】未知

【试题分析】时间复杂度O(n),空间复杂度O(1)

【参考代码】

``` cpp 
int findMoreThanHalf(int array[], int n) {

	if(array == NULL || n <= 0) {
		throw ("Invalid input.");
	}

	int num = array[0];
	int times = 1;

	for(int i = 1; i < n; i++) {
		if(array[i] == num) {
			++times;
		} else {
			--times;
			if(times == 0) {
				num = array[i];
				times = 1;
			}
		}
	}

	return num;
}
```
