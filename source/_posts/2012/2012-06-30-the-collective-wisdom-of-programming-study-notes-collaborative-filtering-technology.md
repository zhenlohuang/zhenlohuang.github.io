---
layout: post
title: '【集体智慧编程 学习笔记】 协同过滤技术'
date: 2012-6-30
wordpress_id: 3213
permalink: /archives/the-collective-wisdom-of-programming-study-notes-collaborative-filtering-technology.html
categories: [Data Mining]
tags: []
keywords: ""
description: 
comments: true
---


协同过滤技术可以分为三类：基于用户（User-based）的协同过滤；基于项目（Item-based）的协同过滤；基于模型（Model-based）的协同过滤。 

**基于用户（User-based）的协同过滤**

用相似统计的方法得到具有相似爱好或者兴趣的相邻用户，所以称之为以用户为基础（User-based）的协同过滤或基于邻居的协同过滤(Neighbor-based Collaborative Filtering)。方法步骤：

1.收集用户资讯

收集可以代表用户兴趣的资讯。一般的网站系统使用评分的方式或是给予评价，这种方式被称为「主动评分」。另外一种是「被动评分」，是根据用户的行为模式由系统代替用户完成评价，不需要用户直接打分或输入评价资料。电子商务网站在被动评分的资料获取上有其优势，用户购买的商品记录是相当有用的资料。

2.最近邻搜索(Nearest neighbor search, NNS)

以用户为基础（User-based）的协同过滤的出发点是与用户兴趣爱好相同的另一组用户，就是计算两个用户的相似度。例如：寻找n个和A有相似兴趣用户，把他们对M的评分作为A对M的评分预测。一般会根据资料的不同选择不同的演算法，目前较多使用的相似度演算法有Person Correlation Coefficient、Cosine-based Similarity、Adjusted Cosine Similarity。

3.产生推荐结果

有了最近邻集合，就可以对目标用户的兴趣进行预测，产生推荐结果。依据推荐目的的不同进行不同形式的推荐，较常见的推荐结果有Top-N推荐和关联推荐。 Top-N推荐是针对个体用户产生，对每个人产生不一样的结果，例如：透过对A用户的最近邻用户进行统计，选择出现频率高且在A用户的评分项目中不存在的，作为推荐结果。关联推荐是对最近邻用户的记录进行关联规则(association rules)挖掘。

**基于项目（Item-based）的协同过滤**

以项目为基础的协同过滤方法有一个基本的假设：“能够引起用户兴趣的项目，必定与其之前评分高的项目相似”，透过计算项目之间的相似性来代替用户之间的相似性。方法步骤：

1.收集用户资讯

同以用户为基础（User-based）的协同过滤。

2.针对项目的最近邻搜索

先计算己评价项目和待预测项目的相似度，并以相似度作为权重，加权各已评价项目的分数，得到待预测项目的预测值。例如：要对项目A和项目B进行相似性计算，要先找出同时对A和B打过分的组合，对这些组合进行相似度计算，常用的演算法同以用户为基础（User-based ）的协同过滤。

3.产生推荐结果

以项目为基础的协同过滤不用考虑用户间的差别，所以精度比较差。但是却不需要用户的历史资料，或是进行用户识别。对于项目来讲，它们之间的相似性要稳定很多，因此可以离线完成工作量最大的相似性计算步骤，从而降低了线上计算量，提高推荐效率，尤其是在用户多于项目的情形下尤为显著。

**基于模型（Model-based）的协同过滤**

以用户为基础（User-based）的协同过滤和以项目为基础（Item-based）的协同过滤统称为以记忆为基础（Memory based）的协同过滤技术，他们共有的缺点是资料稀疏，难以处理大资料量影响即时结果，因此发展出以模型为基础的协同过滤技术。以模型为基础的协同过滤(Model-based Collaborative Filtering)是先用历史资料得到一个模型，再用此模型进行预测。以模型为基础的协同过滤广泛使用的技术包括Latent Semantic Indexing、 Bayesian Networks…等，根据对一个样本的分析得到模型。

参考资料：<http://gengrenjie.com/2009/04/12/%E5%8D%8F%E5%90%8C%E8%BF%87%E6%BB%A4%E6%89%AB%E7%9B%B2%E7%8F%AD%EF%BC%884%EF%BC%89/>