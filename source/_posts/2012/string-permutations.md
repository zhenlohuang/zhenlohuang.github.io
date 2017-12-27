---
layout: post
title: '【IT笔试面试题整理】字符串的排列'
date: 2012-10-6
wordpress_id: 3424
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】输入一个字符串，打印出该字符串中字符的所有排列。例如输入字符串abc，则打印出a，b，c所能排列出来的所有字符串abc，acb，bac，bca，cab，cba。

【试题来源】未知

【参考代码】

``` cpp
#include <iostream>
#include <cstring>
#include<cstdio>
#include<cstdlib>
using namespace std;

void permutation(char* str, int begin) {

	if(begin == strlen(str)) {
		cout << str << endl;
	} else {
		for(int i = begin; i < strlen(str); ++i) {
			char tmp = str[i];
			str[i] = str[begin];
			str[begin] = tmp;

			permutation(str, begin + 1);

			tmp = str[i];
			str[i] = str[begin];
			str[begin] = tmp;
		}
	}
}

void permutation(char* str) {
	if(str == NULL) {
		return ;
	}

	permutation(str, 0);
}

int main() {
	char str[] = "abc";
	//char* str = "abc"; //Runtime Error
	permutation(const_cast<char*>(str));
	return 0;
}
```
