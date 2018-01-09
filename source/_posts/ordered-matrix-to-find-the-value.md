---
layout: post
title: '【IT笔试面试题整理】有序矩阵查找值'
date: 2011-12-29
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】    
在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。（PS：数组的不一定是n*n的矩阵）

【试题来源】未知   
 
【试题分析】    
总体思路就是使用递归+二分查找的方法，具体过程如下所示：
设二分查找的中间点为(m_x,m_y)，其中
m_x = (s_x +e_x) / 2
m_y = (s_y +e_y) / 2
![image](/images/uploads/2011/12/0_13251346359Vmx.gif)
![image](/images/uploads/2011/12/0_1325134639t706.gif)

【源代码Python】    

``` python
#!/usr/bin/env python

def find_matrix(mat, s_x, s_y, e_x, e_y, key):
    """
    mat:传入的矩阵
    (s_x, s_y):矩阵左上角坐标值
    (e_x, e_y):矩阵右下角坐标值
    """
    #明确key不在矩阵中的时候
    if mat[s_x][s_y] > key or mat[e_x][e_y] < key:
        return False

    #当矩阵规模为1*1的时候
    if s_x == e_x and s_y == e_y:
        if mat[s_x][s_y] == key:
            print("(%s, %s)" % (s_x, s_y))
            return True
        else:
            return False

    #中间点坐标值
    m_x = int((s_x + e_x) / 2)
    m_y = int((s_y + e_y) / 2)
    if mat[m_x][m_y] == key:
        print("(%s, %s)" % (s_x, s_y))
        return True
    elif mat[m_x][m_y] > key:
        #查询左上角,右上角,左下角矩阵
        return find_matrix(mat,s_x, s_y, m_x, m_y, key) \
               or find_matrix(mat,s_x, m_y + 1, m_x - 1, e_y, key) \
               or find_matrix(mat,m_x + 1, s_y, e_x, m_y - 1, key)
    elif mat[m_x][m_y] < key:
        #查询左下角,右上角,右下角矩阵
        return find_matrix(mat,m_x + 1, s_y, e_x, m_y, key) \
               or find_matrix(mat,s_x, m_y + 1, m_x, e_y, key) \
               or find_matrix(mat,m_x + 1, m_y + 1, e_x, e_y, key)

def main():
    matrix = [[1, 2, 3 , 4, 5],
              [6, 7, 8 , 9, 10],
              [11, 12, 13, 14, 15],
              [16, 17, 18, 19, 20],
              [21, 22, 23, 24, 25],
              [26, 27, 28, 29, 30],
              [31, 32, 33, 34, 35]]
    key = 24
    if find_matrix(matrix, 0, 0, len(matrix) - 1, len(matrix[0]) - 1, key):
        print("Found!")
    else:
        print("Not Found!")

if __name__ == '__main__':
    main()
```

【参考资料】

<http://topic.csdn.net/u/20111214/10/d09797c3-d1ce-4249-b1e5-8b693b4c85f8.html">http://topic.csdn.net/u/20111214/10/d09797c3-d1ce-4249-b1e5-8b693b4c85f8.html>
<http://justjavac.iteye.com/blog/1310178>
<http://nubnub.blog.163.com/blog/static/169186347201192411857362/>
