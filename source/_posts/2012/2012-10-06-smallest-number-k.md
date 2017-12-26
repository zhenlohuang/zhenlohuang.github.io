---
layout: post
title: '【IT笔试面试题整理】最小K个数'
date: 2012-10-6
wordpress_id: 3428
permalink: /archives/smallest-number-k.html
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】输入n个整数，找出其中最小的k个数。

【试题来源】未知

【参考代码】

``` cpp
void leastKNum(int array[], int n, int k) {

	if(array == NULL || n < k || n <= 0) {
		throw ("Invalid input.");
	}

	priority_queue<int> heap;  //大顶堆

	for(int i = 0; i < n; i++) {

		if(i < k) {
			heap.push(array[i]);
		} else {
			if(array[i] < heap.top()) {
				heap.pop();
				heap.push(array[i]);
			}
		}
	}

	//Print the reslut
	while(!heap.empty()) {
		cout << heap.top() << " ";
		heap.pop();
	}
	cout << endl;
}
```

最大K个数代码如下：

``` cpp
void topKNum(int array[], int n, int k) {

	if(array == NULL || n < k || n <= 0) {
		throw ("Invalid input.");
	}

	priority_queue< int, vector<int>, greater<int> > heap;  //小顶堆

	for(int i = 0; i < n; i++) {

		if(i < k) {
			heap.push(array[i]);
		} else {
			if(array[i] > heap.top()) {
				heap.pop();
				heap.push(array[i]);
			}
		}
	}

	//Print the reslut
	while(!heap.empty()) {
		cout << heap.top() << " ";
		heap.pop();
	}
	cout << endl;
}
```
