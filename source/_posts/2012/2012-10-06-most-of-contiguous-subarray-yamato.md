---
layout: post
title: '【IT笔试面试题整理】连续子数组的最大和'
date: 2012-10-6
wordpress_id: 3432
permalink: /archives/most-of-contiguous-subarray-yamato.html
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】输入一个整型数组，数组里有正数也有负数。数组中一个或连续的多个整数组成一个子数组。

求所有子数组的和的最大值。要求时间复杂度O(n)。

【试题来源】未知

【参考代码】

``` cpp
int findGreastestSumOfSubArray(int array[], int n) {

	if(array == NULL || n <= 0) {
		throw ("Invalid Inptut.");
	}

	int *dp = new int[n];
	int maxSum = array[0];
	for(int i = 0; i < n; i++) {
		if(i == 0 || dp[i - 1] <= 0) {
			dp[i] = array[i];
		} else if(i > 0 && dp[i - 1] > 0){
			dp[i] = dp[i - 1] + array[i];
		}
		if(dp[i] > maxSum) {
			maxSum = dp[i];
		}
	}

	return maxSum;
}
```
