---
layout: post
title: '【IT笔试面试题整理】二叉树的深度'
date: 2012-10-6
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】输入一棵二叉树的根结点，求该树的深度。从根结点到叶子结点依次经过的结点（含根、叶子节点）形成树的一条路径，最长路径的长度为树的深度。

【试题来源】未知

【参考代码】

``` cpp 
int treeDepth(BinaryTreeNode* rootNode) {

	if(rootNode == NULL) {
		return 0;
	}

	int leftSubTreeDepth = treeDepth(rootNode->left);
	int rightSubTreeDepth = treeDepth(rootNode->right);

	return leftSubTreeDepth > rightSubTreeDepth ? (leftSubTreeDepth + 1) : (rightSubTreeDepth + 1);
}
```
