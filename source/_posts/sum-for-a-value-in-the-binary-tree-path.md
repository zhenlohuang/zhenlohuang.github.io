---
layout: post
title: '【IT笔试面试题整理】二叉树中和为某一值的路径'
date: 2012-10-6
wordpress_id: 3421
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】输入一个二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成的一条路径。

【试题来源】未知

【参考代码】

``` cpp
void findPath(BinaryTreeNode* rootNode, int expectSum,
		vector<int>& path, int curSum) {

	if(rootNode == NULL) {
		return ;
	}

	path.push_back(rootNode->value);
	curSum += rootNode->value;

	if(rootNode->left == NULL && rootNode->right == NULL
			&& curSum == expectSum) {
		cout << "Find a path: ";
		for(vector<int>::iterator iter = path.begin(); iter != path.end(); iter++) {
			cout << *iter << " ";
		}
		cout << endl;
	}

	if(rootNode->left != NULL) {
		findPath(rootNode->left, expectSum, path, curSum);
	}

	if(rootNode->right != NULL) {
		findPath(rootNode->right, expectSum, path, curSum);
	}

	path.pop_back();
	curSum -= rootNode->value;
}

void findPaths(BinaryTreeNode* rootNode, int expectSum) {

	vector<int> path;
	findPath(rootNode, expectSum, path, 0);
}
```
