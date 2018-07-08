---
layout: post
title: '【集体智慧编程 学习笔记】 Euclidean距离和Pearson相关系数'
date: 2012-6-24
categories: [Data Mining]
tags: []
keywords: ""
description: 
comments: true
---
*Euclidean距离*

定义:欧几里得空间中点 x = (x1,...,xn) 和 y = (y1,...,yn) 之间的距离为

![image](/images/legacy/2012/06/clip_image001_thumb2-300x33.png)

两个变量之间的相关系数越高，从一个变量去预测另一个变量的精确度就越高，这是因为相关系数越高，就意味着这两个变量的共变部分越多，所以从其中一个变量的变化就可越多地获知另一个变量的变化。如果两个变量之间的相关系数为1或-1，那么你完全可由变量X去获知变量Y的值。

· 当相关系数为0时，X和Y两变量无关系。

· 当X的值增大，Y也增大，正相关关系，相关系数在0.00与1.00之间

· 当X的值减小，Y也减小，正相关关系，相关系数在0.00与1.00之间

· 当X的值增大，Y减小，负相关关系，相关系数在-1.00与0.00之间

当X的值减小，Y增大，负相关关系，相关系数在-1.00与0.00之间

相关系数的绝对值越大，相关性越强，相关系数越接近于1和-1，相关度越强，相关系数越接近于0，相关度越弱。

![image](/images/legacy/2012/06/96202_2375725_1.gif)

实现代码：

``` python
from math import sqrt

# A dictionary of movie critics and their ratings of a small
# set of movies
movies = {'Lisa Rose': {'Lady in the Water': 2.5, 
                       'Snakes on a Plane': 3.5,
                       'Just My Luck': 3.0, 
                       'Superman Returns': 3.5, 
                       'You, Me and Dupree': 2.5, 
                       'The Night Listener': 3.0},
         'Gene Seymour': {'Lady in the Water': 3.0, 
                          'Snakes on a Plane': 3.5, 
                          'Just My Luck': 1.5, 
                          'Superman Returns': 5.0, 
                          'The Night Listener': 3.0, 
                          'You, Me and Dupree': 3.5}, 
         'Michael Phillips': {'Lady in the Water': 2.5, 
                              'Snakes on a Plane': 3.0,
                              'Superman Returns': 3.5, 
                              'The Night Listener': 4.0},
         'Claudia Puig': {'Snakes on a Plane': 3.5, 
                          'Just My Luck': 3.0,
                          'The Night Listener': 4.5,
                          'Superman Returns': 4.0, 
                          'You, Me and Dupree': 2.5},
         'Mick LaSalle': {'Lady in the Water': 3.0, 
                          'Snakes on a Plane': 4.0, 
                          'Just My Luck': 2.0, 
                          'Superman Returns': 3.0, 
                          'The Night Listener': 3.0,
                          'You, Me and Dupree': 2.0}, 
         'Jack Matthews': {'Lady in the Water': 3.0, 
                           'Snakes on a Plane': 4.0,
                           'The Night Listener': 3.0, 
                           'Superman Returns': 5.0, 
                           'You, Me and Dupree': 3.5},
         'Toby': {'Snakes on a Plane':4.5,
                  'You, Me and Dupree':1.0,
                  'Superman Returns':4.0}}

def euclidean(data, p1, p2):
    "Calculate Euclidean distance"
    distance = sum([pow(data[p1][item]-data[p2][item],2) 
                      for item in data[p1] if item in data[p2]])

    return distance

def pearson(data, p1, p2):
    "Calculate Pearson correlation coefficient"    
    corrItems = [item for item in data[p1] if item in data[p2]]

    n = len(corrItems)
    if n == 0: 
        return 0;

    sumX = sum([data[p1][item] for item in corrItems])
    sumY = sum([data[p2][item] for item in corrItems])
    sumXY = sum([data[p1][item] * data[p2][item] for item in corrItems])
    sumXsq = sum([pow(data[p1][item], 2) for item in corrItems])
    sumYsq = sum([pow(data[p2][item],2) for item in corrItems])         

    pearson = (sumXY - sumX * sumY / n) / sqrt((sumXsq - pow(sumX, 2) / n) * (sumYsq - pow(sumY, 2) / n))
    return pearson
```

