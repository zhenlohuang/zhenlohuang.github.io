---
layout: post
title: '【IT笔试面试题整理】二叉排序树的后序遍历'
date: 2012-9-23
wordpress_id: 3417
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】输入一个整数数组，判断该数组是不是某二叉排序树的后序遍历结果。如果是则返回true，否则返回false。假设输入的数组的任意两个数字互不相同。

【试题来源】未知

【参考代码】

``` cpp 
#include <iostream>
using namespace std;

bool verifySquenceOfBST(int sequence[], int length) {

	if(sequence == NULL || length <= 0) {
		return false;
	}

	int root = sequence[length - 1];

	int i = 0;
	while(i < length - 1) {
		if(sequence[i] > root) {
			break;
		}
		i++;
	}

	int j = i;
	while(j < length - 1) {
		if(sequence[j] < root) {
			return false;
		}
		j++;
	}

	//判断左子数
	bool left = true;
	if(i > 0) {
		left = verifySquenceOfBST(sequence, i);
	}

	//判断右子数
	bool right = true;
	if(i > 0) {
		right = verifySquenceOfBST(sequence + i, length - i - 1);
	}

	return left && right;
}

int main() {
	int seq[] = {5, 7, 6, 9, 11, 10, 8};
	//int seq[] = {5, 7, 6, 11, 9, 10, 8};
	int length = 7;

	if(verifySquenceOfBST(seq, length)) {
		cout << "Yes" << endl;
	} else {
		cout << "No" << endl;
	}
	return 0;
}
```
