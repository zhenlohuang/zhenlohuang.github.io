---
layout: post
title: '机器学习 支持向量机(SVM)'
date: 2010-10-23
wordpress_id: 479
permalink: /archives/machine-learning-support-vector-machine-svm.html
categories: [Machine Learning]
tags: [SVM]
keywords: "SVM"
description: 
comments: true
---
SVM方法的基本思想是：定义最优线性超平面，并把寻找最优线性超平面的算法归结为求解一个凸规划问题。进而基于Mercer核展开定理，通过非线性映射φ，把样本空间映射到一个高维乃至于无穷维的特征空间（Hilbert空间），使在特征空间中可以应用线性学习机的方法解决样本空间中的高度非线性分类和回归等问题。
 
使用工具：  
1）Libsvm：<http://www.csie.ntu.edu.tw/~cjlin/libsvm/>   
2）python：版本为2.6.x或2.7都可以   
3）gnuplot：<http://www.gnuplot.info/>  
 
数据样本下载：<http://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/>  
（建议成对下载训练样本和测试样本）   

配置环境：  
将一下变量添加到环境变量中：  
C:/Program Files/Python27;  
C:/gnuplot/binary;  
C:/libsvm-3.0/libsvm-3.0/windows  
 
 
1、  利用svm-train训练样本并生成训练模型  
Dos中输入：svm-train.exe –c 512 –g 0.002 data1.train  
![image1](/images/uploads/2010/10/0_1287830139929O.gif)  
将生成一个后缀为model的文件  
![image2](/images/uploads/2010/10/0_1287830179zPX1.gif)

2、  利用svm-predict预测测试数据  
在dos中输入：```svm-predict.exe data1.testing data1.train.model data1.out```
![image3](/images/uploads/2010/10/0_1287830185Oo7L.gif)

3、  使用grid.py获取最佳参数  
这边令参数c为512，g为0.002并不是最佳参数，后面将介绍使用grid.py来获取最佳参数  
转到tools目录下cd C:/libsvm-3.0/libsvm-3.0/tools  
打开grid.py修改gnuplot_exe的路径，将路径改为和你的gnuplot路径一致  
gnuplot_exe = r"C:/gnuplot/binary/pgnuplot.exe"  
在dos中执行：python grid.py data1.train  
等个几分钟就会获得一个最佳参数并生成一张分析过程图片  
 
4、  使用easy.py一步到位  
要是觉得前面的过程过于繁琐的话，可以使用easy.py进行直接处理  
首先还是修改gnuplot_exe路径  
然后在dos中执行:```python easy.py data1.train data1.testing```

![image4](/images/uploads/2010/10/0_1287830189YC11.gif)
