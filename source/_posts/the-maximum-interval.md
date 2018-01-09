---
layout: post
title: '【IT笔试面试题整理】最大间隔问题'
date: 2010-10-17
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
**【题目描述】**给定n个实数x1,x2,...,xn,求这n个实数在实轴上相邻2个数之间的最大差值,要求设计线性的时间算法 
  
**【题目来源】**百度    

**【题目分析】**  
由于要求要线性时间，所以不能使用排序，于是使用了一种类似桶排序的变形  

**【算法】**  
距离平均值为offset =(arrayMax - arrayMin) / (n - 1),
则距离最大的数必然大于这个值。
每个桶只要记住桶中的最大值和最小值，依次比较上一个桶的最大值与下一个桶的最小值的差值，找最大的即可.

``` cpp 
/**
 给定n个实数x1,x2,...,xn,求这n个实数在实轴上相邻2个数之间的最大差值,要求设计线性的时间算法
 */
 /**
 算法：
 距离平均值为offset = (arrayMax - arrayMin) / (n - 1), 则距离最大的数必然大于这个值
 每个桶只要记住桶中的最大值和最小值，依次比较上一个桶的最大值与下一个桶的最小值的差值
 找最大的即可.
 */
 #include <iostream>
 #define MAXSIZE 100 //实数的个数
 #define MAXNUM 32767
 using namespace std;
 struct Barrel
 {
 double min; //桶中最小的数
 double max; //桶中最大的数
 bool flag; //标记桶中有数
 };
 int BarrelOperation(double* array, int n)
 {
 Barrel barrel[MAXSIZE]; //实际使用的桶
 int nBarrel = 0; //实际使用桶的个数
 Barrel tmp[MAXSIZE]; //临时桶，用于暂存数据
 double arrayMax = -MAXNUM, arrayMin = MAXNUM;
 for(int i = 0; i < n; i++) {
 if(array[i] > arrayMax)
 arrayMax = array[i];
 if(array[i] < arrayMin)
 arrayMin = array[i];
 }
 double offset = (arrayMax - arrayMin) / (n - 1); //所有数的平均间隔
 //对桶进行初始化
 for(i = 0; i < n; i++) {
 tmp[i].flag = false;
 tmp[i].max = arrayMin;
 tmp[i].min = arrayMax;
 }
 //对数据进行分桶
 for(i = 0; i < n; i++) {
 int pos = (int)((array[i] - arrayMin) / offset);
 if(!tmp[pos].flag) {
 tmp[pos].max = tmp[pos].min = array[i];
 tmp[pos].flag = true;
 } else {
 if(array[i] > tmp[pos].max)
 tmp[pos].max = array[i];
 if(array[i] < tmp[pos].min)
 tmp[pos].min = array[i];
 }
 }
 for(i = 0; i <= n; i++) {
 if(tmp[i].flag)
 barrel[nBarrel++] = tmp[i];
 }
 int maxOffset = 0.0;
 for(i = 0; i < nBarrel - 1; i++) {
 if((barrel[i+1].min - barrel[i].max) > maxOffset)
 maxOffset = barrel[i+1].min - barrel[i].max;
 }
 return maxOffset;
 }
 int main()
 {
 double array[MAXSIZE] = {1, 8, 6, 11, 7, 13, 16, 5}; //所需处理的数据
 int n = 8; //数的个数
 //double array[MAXSIZE] = {8, 6, 11};
 //int n = 3;
 int maxOffset = BarrelOperation(array, n);
 cout << maxOffset << endl;
 return 0;
 }
```
【参考资料】
<http://topic.csdn.net/u/20101015/15/1eaab1a3-4432-4f17-be8b-1716fef3e59c.html>
<http://blog.csdn.net/livelylittlefish/archive/2008/03/23/2209537.aspx>