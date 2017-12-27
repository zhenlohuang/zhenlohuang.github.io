---
layout: post
title: '【IT笔试面试题整理】用两个栈实现队列'
date: 2012-9-6
wordpress_id: 3294
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】用两个栈实现一个队列。队列声明如下，请实现它的两个函数appendTail，deleteHead，分别完成在队列尾部插入节点和在头部删除节点。

【试题来源】未知

【参考代码】

``` cpp 
#include <iostream>
#include <stack>
using namespace std;

template <typename T>
class MyQueue
{

public:
	void appendTail(const T& element);
	T deleteHead();

private:
	stack<T> inStack;
	stack<T> outStack;
};

template <typename T>
void MyQueue<T>::appendTail(const T& element) {

	inStack.push(element);
}

template <typename T>
T MyQueue<T>::deleteHead() {

	if(outStack.empty()) {
		//将inStack里面的数据放入outStack中
		if(!inStack.empty()) {
			while(!inStack.empty()) {
				T element = inStack.top();
				outStack.push(element);
				inStack.pop();
			}
		} else {
			throw "The Queue is empty.";
		}
	}
	T element = outStack.top();
	outStack.pop();

	return element;
}

int main() {
	MyQueue<int> queue;
	queue.appendTail(1);
	queue.appendTail(3);
	queue.appendTail(5);
	cout << queue.deleteHead() << endl;
	cout << queue.deleteHead() << endl;
	cout << queue.deleteHead() << endl;

	return 0;
}
```
