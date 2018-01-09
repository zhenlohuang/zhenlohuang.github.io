---
layout: post
title: '【IT笔试面试题整理】替换空格'
date: 2012-9-4
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】请实现一个函数，把字符串中的每个空格替换成%20。例如输入"We are happy."，则输出"We%20are%20happy."。（不能使用任何字符串操作函数）

【试题来源】未知

【试题分析】由于不能使用字符串操作函数，因此只能一个字符一个字符的进行处理。 首先先遍历一遍字符串，统计得到所有的空格，那么替换后的字符串长度应 为len(str) + numOfBlank * 2。然后从后往前面修改字符串。算法时间复杂度O(n)。

【参考代码】

``` cpp 
#include <cstdio>
#include <cstring>
using namespace std;

void replaceSpace(char str[], int length) {
	if(str == NULL) {
		return ;
	}

	int numOfBlank = 0;
	int i = 0;
	while(str[i] != '\0') {
		if(str[i] == ' ') {
			numOfBlank++;
		}
		i++;
	}

	int indexOriginalString = length - 1;
	int indexNewString = length + numOfBlank * 2 - 1;
	while(indexOriginalString >= 0) {
		if(str[indexOriginalString] == ' ') {
			str[indexNewString--] = '0';
			str[indexNewString--] = '2';
			str[indexNewString--] = '%';
		} else {
			str[indexNewString--] = str[indexOriginalString];
		}
		indexOriginalString--;
	}
}

int main() {
	char str[] = "We are happy.";
	//char str[] = " We are happy. ";
	//char str[] = "Wearehappy.";
	replaceSpace(str, strlen(str));
	printf("%s", str);

	return 0;
}
```
