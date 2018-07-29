---
layout: post
title: '【IT笔试面试题整理】丑数'
date: 2012-10-6
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】我们把只包含因子2、3和5的数称作丑数。求按从到大的顺序的第1500个丑数。例如6,8是丑数，而14不是，因为它包含因子7.习惯上把1当作第一个丑数。

【试题来源】未知

【参考代码】

``` cpp
int Min(int a, int b, int c) {

	return (a < b ? a : b) < c ? (a < b ? a : b) : c;
}

int uglyNumber(int n) {

	if(n <= 0) {
		throw ("Invalid Input.");
	}

	long* uglyNum = new long[n];
	uglyNum[0] = 1;

	int index2 = 0;
	int index3 = 0;
	int index5 = 0;
	int indexLast = 0;

	while(indexLast < n) {
		int min = Min(uglyNum[index2] * 2, uglyNum[index3] * 3, uglyNum[index5] * 5);
		uglyNum[++indexLast] = min;

		while(uglyNum[index2] * 2 <= uglyNum[indexLast]) {
			index2++;
		}

		while(uglyNum[index3] * 3 <= uglyNum[indexLast]) {
			index3++;
		}

		while(uglyNum[index5] * 5 <= uglyNum[indexLast]) {
			index5++;
		}
	}

	return uglyNum[n - 1];

}
```
