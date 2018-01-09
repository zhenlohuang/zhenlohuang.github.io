---
layout: post
title: '【IT笔试面试题整理】1到N自然数排序'
date: 2010-9-16
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【题目描述】1到N自然数排序。要求时间复杂度为O(n)，空间复杂度为O(1)

【题目来源】华为

【题目分析】

【代码】

``` cpp 
/**
  1到N自然数排序(华为面试题)
  要求：时间复杂度为O(n)，空间复杂度为O(1)
*/
#include <iostream>
using namespace std;
int array[10] = {0, 2, 4, 6, 9, 8, 1, 3, 7, 5};
int n = 10;
void sort()
{
	int t;
	for(int i = 0; i < n; i++)
	{
		while(array[i] != i)
		{
			t = array[array[i]];
			array[array[i]] = array[i];
			array[i] = t;
		}
	}
}
void output()
{
	for(int i = 0; i < 10; i++)
		cout << array[i];
	cout << endl;
}
int main()
{
	sort();
	output();
	return 0;
}
```
