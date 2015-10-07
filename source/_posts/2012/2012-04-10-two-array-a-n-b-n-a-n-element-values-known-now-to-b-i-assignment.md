---
layout: post
title: '【IT笔试面试题整理】两个数组a[N]，b[N]，其中A[N]的各个元素值已知，现给b[i]赋值'
date: 2012-4-10
wordpress_id: 540
permalink: /archives/two-array-a-n-b-n-a-n-element-values-known-now-to-b-i-assignment.html
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---


【题目描述】
两个数组a[N]，b[N]，其中A[N]的各个元素&#20540;已知，现给b[i]赋&#20540;，b[i]
 = a[0]*a[1]*a[2]...*a[N-1]/a[i]；
要求：
1.不准用除法运算
2.除了循环计数&#20540;，a[N],b[N]外，不准再用其他任何变量（包括局部变量，全局变量等）
3.满足时间复杂度O（n），空间复杂度O（1）

【题目来源】腾讯2012

【题目分析】
由于题目要求甚多，就必须充分使用现有资源数组b，具体分析如下：
b[0] = a[1] *a[2] * a[3] …… * a[N-2] * a[N-1]
b[1] = a[0] * a[2]* a[3] …… * a[N-2] * a[N-1]
b[2] = a[0] * a[1]* a[3] …… * a[N-2] * a[N-1]

b[N-2] =a[0] * a[1]* a[2] * a[3] …… * a[N-3] * a[N-1]
b[N-1] =a[0] * a[1]* a[2] * a[3] …… * a[N-2]
由上面可以推得
b[i] = <span style="background:#D9D9D9">a[0] *a[1] * a[2] … a[i-1] * <span style="background:#D9D9D9">
a[i+1] * a[i+2] * … a[N-2] * a[N -1]
因此可以采用分两段计算的策略解决问题。

【代码】

``` cpp
#include<iostream>
using namespace std;

#define N 10

int main()
{
	int a[N] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
	int b[N] = {};

	b[0] = 1;
	for(int i = 1; i < N; i++) {
		b[0] *= a[i-1];
		b[i] = b[0];
	}
	b[0] = 1;
	for(int i = N - 2; i > 0; i--) {
		b[0] *= a[i+1];
		b[i] *= b[0];
	}
	b[0] *= a[1];

	//Test
	for(int i = 0; i < N; i++) {
		cout << b[i] << " ";
	}
	cout << endl;
}
```

【运行结果】    
![image](/images/uploads/2012/04/1334034201_4639.png)

【参考资料】    
<http://topic.csdn.net/u/20120407/17/2debad5f-d37a-4b41-ab8a-cab309910ccd.html>

