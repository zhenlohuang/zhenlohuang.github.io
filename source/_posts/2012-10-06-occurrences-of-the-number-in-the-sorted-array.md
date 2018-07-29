---
layout: post
title: '【IT笔试面试题整理】数字在排序数组中出现的次数'
date: 2012-10-6
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】统计一个数字在排序数组中出现的次数。例如输入的排序数组{1, 2, 3, 3, 3, 3, 4, 5}

和数字3,由于3在这个数组中出现了4次，因此输出4。

【试题来源】未知

【参考代码】

``` cpp
int GetFirstK(int array[], int length, int start, int end, int k) {

	if(start > end) {
		return -1;   //No Found
	}

	int middle = (start + end) / 2;
	if(array[middle] == k) {

		if((middle > 0 && array[middle - 1] != k)
				|| middle == 0) {
			return middle;
		} else {
			end = middle - 1;
		}
	} else if(array[middle] < k) {
		start = middle + 1;
	} else {
		end = middle - 1;
	}

	return GetFirstK(array, length, start, end, k);
}

int GetLastK(int array[], int length, int start, int end, int k) {

	if(start > end) {
		return -1;   //No Found
	}

	int middle = (start + end) / 2;
	if(array[middle] == k) {

		if((middle < length - 1 && array[middle + 1] != k)
				|| middle == length - 1) {
			return middle;
		} else {
			start = middle + 1;
		}
	} else if(array[middle] < k) {
		start = middle + 1;
	} else {
		end = middle - 1;
	}

	return GetLastK(array, length, start, end, k);
}

int GetNumOfK(int array[], int length, int k) {

	if(array == NULL || length <= 0) {
		throw ("Invalid input.");
	}

	int first = GetFirstK(array, length, 0 , length - 1, k);
	int last = GetLastK(array, length, 0 , length - 1, k);

	if(first > -1 && last > -1) {
		return last - first + 1;
	}

	return -1;
}
```
