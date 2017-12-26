---
layout: post
title: '【IT笔试面试题整理】求1+2+…+n'
date: 2011-4-6
wordpress_id: 502
permalink: /archives/add-one-to-n.html
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【题目描述】
求1+2+…+n
要求不能使用乘除法、for、while、if、else、switch、case
等关键字以及条件判断语句
 
【题目来源】未知
 
【题目分析】
一般求1+2+…n，是使用循环或者阶乘，这边加限制条件可以考虑用递归。如果使用递归的话，最重要的就是考虑递归退出条件，由于不能用if语句，而退出必然需要判断，于是使用了bool型表达式，用于终止函数的继续递归。
 
【代码】

``` cpp 
#include<iostream>
using namespace std;
int sum(int n)
{
int res = 0;
int i = 1;
(n > 0)&&(res = sum(n-1) + n);
return res;
}
int main()
{
//Just For Test
int res = sum(100);
cout << res <<endl;
return 0;
}
```
