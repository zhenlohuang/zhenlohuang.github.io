---
layout: post
title: '【IT笔试面试题整理】旋转数组的最小数字'
date: 2012-9-6
wordpress_id: 3296
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】把一个数组最开始的若干个元素搬到末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的小元素。例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小元素为1.

【试题来源】未知

【参考代码】

``` cpp 
#include <iostream>
using namespace std;

int findMinNumber(int* numbers, int length) {

	if(numbers == NULL || length <= 0) {
		throw "Invalid parameters.";
	}

	int index1 = 0;
	int index2 = length - 1;
	int indexMid = (index1 + index2) / 2;
	while(numbers[index1] >= numbers[index2]) {

		if(index2 - index1 == 1) {
			return numbers[index2];
		}

		indexMid = (index1 + index2) / 2;
		if(numbers[index1] == numbers[index2]
		    && numbers[index1] == numbers[indexMid]) {
			int result = numbers[index1 + 1];
			for(int i = index1 + 1; i <= index2; ++i) {
				if(result > numbers[i]) {
					result = numbers[i];
				}
			}
		} else if(numbers[indexMid] >= numbers[index1]) {
			index1 = indexMid;
		} else if(numbers[indexMid] <= numbers[index2]) {
			index2 = indexMid;
		}
	}
	return numbers[indexMid];
}

int main() {
	int array[] = {3, 4, 5, 1, 2};
	//int array[] = {3, 4, 4, 5, 5, 6, 7, 8, 1, 2, 2, 2, 3, 3, 3};
	cout << findMinNumber(array, 5) << endl;
	return 0;
}
```
