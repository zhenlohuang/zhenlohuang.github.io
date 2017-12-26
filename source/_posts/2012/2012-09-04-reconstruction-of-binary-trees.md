---
layout: post
title: '【IT笔试面试题整理】重建二叉树'
date: 2012-9-4
wordpress_id: 3289
permalink: /archives/reconstruction-of-binary-trees.html
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】输入二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设遍历结果中都不包含重复数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历{4,7,2,1,5,3,8,6}，则重建出该二叉树，并以后续遍历输出。

【参考代码】

``` cpp 
#include <iostream>
using namespace std;

struct BinaryTreeNode
{
	int value;
	BinaryTreeNode* left;
	BinaryTreeNode* right;
};

BinaryTreeNode* rebulidBinaryTreeCore(int* startPreOrder, int* endPreOrder,
		int* startInOrder, int* endInOrder) {

	//先序遍历的第一个节点为根节点
	//创建根节点
	BinaryTreeNode* rootNode = new BinaryTreeNode();
	rootNode->value = startPreOrder[0];
	rootNode->left = NULL;
	rootNode->right = NULL;

	if(startPreOrder == endPreOrder) {
		if(startInOrder == endInOrder
				&& *startPreOrder == *startInOrder) {
			return rootNode;
		} else {
			throw ("Invalid input.");
		}
	}

	//在中序查找根节点
	int* rootIndex = startInOrder;
	while(rootIndex <= endInOrder && *rootIndex != rootNode->value) {
		rootIndex++;
	}
	if(rootIndex == endInOrder && *rootIndex != rootNode->value) {
		throw ("Invalid input.");
	}

	//重建左子树
	int leftTreeLength = rootIndex - startInOrder;
	if(leftTreeLength > 0) {
		rootNode->left = rebulidBinaryTreeCore(startPreOrder + 1,
				startPreOrder + leftTreeLength,
				startInOrder,
				rootIndex - 1);
	}

	//重建右子树
	if(leftTreeLength < endPreOrder - startPreOrder) {
		rootNode->right = rebulidBinaryTreeCore(startPreOrder + leftTreeLength + 1,
				endPreOrder,
				rootIndex + 1,
				endInOrder);
	}

	return rootNode;
}

BinaryTreeNode* rebulidBinaryTree(int* preOrder, int* inOrder, int n) {
	if(preOrder == NULL || inOrder == NULL || n <= 0) {
		return NULL;
	}

	return rebulidBinaryTreeCore(preOrder, preOrder + n -1,
			inOrder, inOrder + n - 1);
}

void printPostOrder(BinaryTreeNode* root) {
	if(root!= NULL) {
		if(root->left != NULL) {
			printPostOrder(root->left);
		}
		if(root->right != NULL) {
			printPostOrder(root->right);
		}
		cout << root->value << " ";
	}
	return ;
}

int main() {
	int preOrder[8] = {1,2,4,7,3,5,6,8};
	int inOrder[8] = {4,7,2,1,5,3,8,6};
	BinaryTreeNode* root = rebulidBinaryTree(preOrder, inOrder, 8);
	printPostOrder(root);
	return 0;
}
```
