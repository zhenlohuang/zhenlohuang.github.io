---
layout: post
title: '【IT笔试面试题整理】调整数组顺序使得奇数位于偶数前面'
date: 2012-9-11
wordpress_id: 3397
permalink: /archives/to-resize-an-array-makes-an-odd-number-in-the-order-even-before.html
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】 输入一个整数数组，实现一个函数来调整该数组中数字的顺序。使得所有奇数位于数组的前半部分，所有偶数位于数组后半部分。

【试题来源】未知

【参考代码】

``` cpp 
#include <iostream>
using namespace std;

void reorder(int* data, int length) {

	int i = 0;
	int j = length - 1;

	while(i < j) {
		while(i < j && (data[i] & 1)) {
			i++;
		}

		while(i < j && !(data[j] & 1)) {
			j--;
		}

		if(i < j) {
			int tmp = data[i];
			data[i] = data[j];
			data[j] = tmp;
		}
	}
}

int main() {
	int data[] = {1,2,3,4,5,6,7,8,9,10};
	int length = 10;
	reorder(data, length);

	for(int i = 0; i < length; i++) {
		cout << data[i] << " ";
	}

	return 0;
}
```
