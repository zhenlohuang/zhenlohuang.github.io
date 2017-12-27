---
layout: post
title: '【IT笔试面试题整理】连续整数之和为1000'
date: 2011-3-21
wordpress_id: 498
permalink: /archives/consecutive-integers-is-1000.html
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【题目描述】连续整数之和为1000可分为几组

【题目来源】Microsoft

【题目分析】
假设连续的整数之和为从n到m。那么n累加到m的和为（n+m）(m-n+1)/2=1000。   
即（n+m）(m-n+1) = 2000。也就是说要将2000分解为一个奇数和偶数的乘积。   
将2000因式分解得到2000 = 2^4 * 5^3。   
于是可以分为4组   
2000 = 16*125   
2000 = 80 * 25   
2000 = 400 * 5   
2000 = 2000 * 1   
解二元一次方程可以得到各个n和m。