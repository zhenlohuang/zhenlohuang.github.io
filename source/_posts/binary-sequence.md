---
layout: post
title: '【IT笔试面试题整理】二进制序列'
date: 2011-4-17
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】    
给你一个二进制序列比如10100，进行左循环移位n   
次，n是二进制序列的长度，每移位一次产生一个新二进制序列   
序列1   
10100   
01001   
10010   
00101   
01010   
然后把这些序列进行排序，变成序列2   

序列2   
00101   
01001   
01010   
10010   
10100   
提取最后一列(注意是列不是行):11000   
现在给你序列2的最后一列11000，让我们求出序列2的第一行   
 
【试题来源】某大学研究生复试面试题   
 
【试题分析】   
首先经过n次变化然后排序所得序列2是个n*n的矩阵M。M矩阵的每一行无论是同时左移或者同时右移I位后得到的新矩阵P，这个新的矩阵的每一行其实还是能在矩阵M中找到。因此，任意一列都包含了原序列的所有信息。   
这里我们有最后一列，只要将最后一列排序即可得到第一列的元素。   
这样我们就可以知道第一列F和最后一列L这时将矩阵每一位循环右移，则此时的矩阵第一列为L，第二列为F。   
然后按照前两列组成的序列进行排序，得到新的矩阵。所得矩阵的第一列与原矩阵第一列相同，即为F，于是第二列为原矩阵的第二列，于是可以求得第二列。   
此时就得到的第一列，第二列，以及已知的最后一列，然后再次右移排序，可以得到第三列。   
经过数次变换可以得到整个矩阵M，即可求得序列2的第一行。   
 
【算法】   
假设变换2的最后一列记为L（实现上可以作为1维数组）   
1. 建立一个n*n的矩阵M（实现上可以作为2维数组），初始值为全0   
2. 将L复制到M的第一列（最左边的一列）   
3. 对M的第一列进行排序（从小到大）   
4. 循环n-2次{将M的每行右移1位；将L复制到M的第一列；将M按行排序}   
5. 将L复到M的最后1列（最右边的一列）   
此时的M就是变化2后的矩阵。   
【代码】   

``` python 
#-------------------------------------------------------------------------------
# Name:            BinarySequence.py
# Purpose:
#
# Author:         Killua
#
# Created:       17/04/2011
# Copyright:    (c) Killua 2011
# Licence:       <your licence>
#-------------------------------------------------------------------------------
#!/usr/bin/env python
def BinarySequence(lastColumn):
      n = len(lastColumn);
      M = ['0'*n for i in range(n)]; #初始化一个空矩阵
      #将最后一列赋值给矩阵
      for i in range(n):
            M[i] = M[i][:n-1] + lastColumn[i];
      #循环移位排序n-1次
      for i in range(n-1):
            #循环移位
            for j in range(n):
                  M[j] = lastColumn[j] + M[j][:n-1];
            #排序
            M.sort();
            #最后一列赋回
            for k in range(n):
                  M[k] = M[k][:n-1] + lastColumn[k];
      return M[0];
def main():
      #Test
      lastColumn = "11000";
      res = BinarySequence(lastColumn);
      print("排序后的矩阵第一行:" + res);
if __name__ == '__main__':
      main()
```

【参考资料】
<http://topic.csdn.net/u/20110405/13/9393c9a7-b86c-482d-acaf-e8c391541875.html>
<http://topic.csdn.net/u/20110405/13/385ab3c5-a21b-407e-9773-a667d327d0ee.html?41862>
<http://en.wikipedia.org/wiki/Burrows%E2%80%93Wheeler_transform>