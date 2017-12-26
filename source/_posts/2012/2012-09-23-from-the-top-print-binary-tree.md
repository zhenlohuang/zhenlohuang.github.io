---
layout: post
title: '【IT笔试面试题整理】从上往下打印二叉树'
date: 2012-9-23
wordpress_id: 3415
permalink: /archives/from-the-top-print-binary-tree.html
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】从上往下打印二叉树

【试题来源】未知

【参考代码】

``` cpp 
void printBinaryTree(const BinaryTreeNode* rootNode) {

	if(rootNode == NULL) {
		return ;
	}

	queue<const BinaryTreeNode*> nodesQueue;
	nodesQueue.push(rootNode);
	while(!nodesQueue.empty()) {
		const BinaryTreeNode* node = nodesQueue.front();
		cout << node->value << " ";
		nodesQueue.pop();

		if(node->left != NULL) {
			nodesQueue.push(node->left);
		}
		if(node->right != NULL) {
			nodesQueue.push(node->right);
		}
	}
}
```
