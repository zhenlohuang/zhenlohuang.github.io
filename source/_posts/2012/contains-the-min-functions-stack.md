---
layout: post
title: '【IT笔试面试题整理】包含min函数的栈'
date: 2012-9-23
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】定义的数据结构，请在类型中实现一个能够得到栈的最小元素的函数。 在该栈中，调用min、push、pop的时间复杂度都是O(1)。

【试题来源】未知

【参考代码】

``` cpp 
#include <iostream>
#include <stack>
using namespace std;

template<typename T>
class StackWithMin {

private:
	stack<T> dataStack;
	stack<T> minStack;

public:
	void push(const T &element);
	T pop();
	T min();
};

template<typename T>
void StackWithMin<T>::push(const T &element) {

	dataStack.push(element);

	if(minStack.empty()) {
		minStack.push(element);
	} else if(element < minStack.top()) {
		minStack.push(element);
	} else {
		minStack.push(minStack.top());
	}
}

template<typename T>
T StackWithMin<T>::pop() {

	if(dataStack.empty()) {
		throw "Stack is empty.";
	}

	T topValue = dataStack.top();
	dataStack.pop();
	minStack.pop();

	return topValue;
}

template<typename T>
T StackWithMin<T>::min() {

	if(dataStack.empty()) {
		throw "Stack is empty.";
	}

	return minStack.top();
}


int main() {
	StackWithMin<int> stackWithMin;
	stackWithMin.push(3);
	stackWithMin.push(4);
	stackWithMin.push(2);
	stackWithMin.push(6);
	cout << stackWithMin.min() << endl;
	stackWithMin.pop();
	stackWithMin.pop();
	cout << stackWithMin.min() << endl;
	return 0;
}
```
