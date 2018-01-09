---
layout: post
title: '各种内排序算法（Python实现）'
date: 2011-4-13
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---

``` python
#-------------------------------------------------------------------------------
# Name:        Sort.py
# Purpose:
#
# Author:      Killua
# E-mail:      killua_hzl@163.com
# Created:     11-04-2011
# Copyright:   (c) Killua 2011
#-------------------------------------------------------------------------------
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import sys;
import random;
import time;
#冒泡排序
def BubbleSort(array):
    for i in range(0,len(array)):
        flag = True;
        for j in range(i,0,-1):
            if array[j] < array[j-1]:
                array[j-1], array[j] = array[j], array[j-1];
                flag = False;
            if flag :
                break;
#选择排序
def SelectSort(array):
    for i in range(0, len(array)):
        pMin = i;
        for j in range(i+1, len(array)):
            if array[j] < array[pMin]:
                pMin = j;
        if pMin != i :
            array[i], array[pMin] = array[pMin], array[i];
#插入排序
def InsertSort(array):
    for i in range(1, len(array)):
        for j in range(i,0,-1):
            if array[j] < array[j-1]:
                array[j],array[j-1] = array[j-1],array[j];
            else:
                break;
#快速排序
def partition(array, left, right):
    mid = (left + right) / 2;
    tmp = array[mid];
    i = left;
    j = right;
    while(True):
        if i == j:
            array[i] = tmp;
            return i;
        if array[i] > tmp:
            array[i] = array[j];
            j = j - 1;
            break;
        i = i + 1;
    while(True):
        if i == j:
            array[i] = tmp;
            return i;
        if array[j] < tmp:
            array[j] = array[i];
            i = i - 1;
            break;
        j = j - 1;
def quickSort(array, left, right):
    if left >= right:
        pivot = partition(array, left, right);
        quickSort(array,left, pivot - 1);
        quickSort(array, pivot + 1, right);
def QuickSort(array):
    quickSort(array, 0, len(array) - 1);
#希尔排序
def ShellSort(array):
    delta = int(len(array) / 2);
    while delta > 0:
        for i in range(0, delta):
            for j in range(i + delta, len(array), delta):
                for k in range(j, 0, -delta):
                    if array[k] < array[k-delta]:
                        array[k],array[k-delta] = array[k-delta],array[k];
        delta = int(delta / 2);
#归并排序
def merge(array, left, right, middle):
    i = left;
    j = middle + 1;
    tmp = [];
    while i <= middle and j <= right:
        if array[i] < array[j]:
            tmp.append(array[i]);
            i = i + 1;
        else:
            tmp.append(array[j]);
            j = j + 1;
    while i <= middle:
        tmp.append(array[i]);
        i = i + 1;
    while j <= right:
        tmp.append(array[j]);
        j = j + 1;
    array[left : right] = tmp[0 : -1]; #此处不能写成array[left : right] = tmp;
def mergeSort(array, left, right):
    if left < right:
        middle = int((left + right) / 2);
        mergeSort(array, left, middle);
        mergeSort(array, middle + 1, right);
        merge(array, left, right, middle);
def MergeSort(array):
    mergeSort(array, 0, len(array) - 1);
#堆排序
def heapAdjust(array, s, e):
    tmp = array[s];
    i = s * 2;
    while i <= e:
        if i + 1 <= e and array[i] < array[i+1]:
            i = i + 1;
        if array[i] < array[s]:
            array[s] = array[i];
            i = s;
        else:
            break;
        i = i * 2;
    array[s] = tmp;
def HeapSort(array):
    for i in (int((len(array) - 1)/2), 0, -1):
        heapAdjust(array, i, len(array) - 1);
    for i in (len(array) - 1, 0, -1):
        array[i], array[0] = array[0], array[i];
        heapAdjust(array, 0, i);
#数据生成
def RandomNumGenerate(n):
    data = [];
    for i in range(0,n):
        data.append(random.randint(0,1000));
    return data;
```

