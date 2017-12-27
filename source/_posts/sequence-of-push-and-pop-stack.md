---
layout: post
title: '【IT笔试面试题整理】栈的压入、弹出序列'
date: 2012-9-23
wordpress_id: 3409
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出序列。

【试题来源】未知

【参考代码】

``` cpp 
#include <iostream>
#include <stack>
using namespace std;

bool isPopOrder(const int* pushOrder, const int* popOrder, int length) {

	if(pushOrder == NULL || popOrder == NULL || length <= 0) {
		return false;
	}

	int i = 0;
	int j = 0;
	stack<int> tmpStack;

	tmpStack.push(pushOrder[i++]);

	while(i < length || j < length) {

		if(tmpStack.top() != popOrder[j]) {
			tmpStack.push(pushOrder[i]);
			i++;
		} else {
			if(!tmpStack.empty()) {
				tmpStack.pop();
			} else {
				return false;
			}
			j++;
		}
	}

	if(tmpStack.empty()) {
		return true;
	} else {
		return false;
	}
}

int main() {
	const int pushOrder[] = {1, 2, 3, 4, 5};
	//const int popOrder[] = {4, 5, 3, 2, 1};
	const int popOrder[] = {4, 3, 5, 1, 2};
	int length = 5;

	if(isPopOrder(pushOrder, popOrder, length)) {
		cout << "Yes" << endl;
	} else {
		cout << "No" << endl;
	}

	return 0;
}
```
