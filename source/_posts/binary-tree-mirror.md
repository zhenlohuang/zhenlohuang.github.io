---
layout: post
title: '【IT笔试面试题整理】二叉树镜像'
date: 2012-9-23
wordpress_id: 3405
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】完成一个函数，输入一个二叉树输出它的镜像。

【试题来源】未知

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

void mirrorRecursively(BinaryTreeNode* rootNode) {

	if(rootNode == NULL || (rootNode->left == NULL && rootNode->right)) {
		return ;
	}

	BinaryTreeNode* tempNode = rootNode->left;
	rootNode->left = rootNode->right;
	rootNode->right = tempNode;

	if(rootNode->left != NULL) {
		mirrorRecursively(rootNode->left);
	}

	if(rootNode->right != NULL) {
		mirrorRecursively(rootNode->right);
	}
}

void printInOrder(BinaryTreeNode* root) {
	if(root!= NULL) {
		cout << root->value << " ";
		if(root->left != NULL) {
			printInOrder(root->left);
		}
		if(root->right != NULL) {
			printInOrder(root->right);
		}
	}
	return ;
}

int main() {
	//建立一颗二叉树
	int preOrder[8] = {1,2,4,7,3,5,6,8};
	int inOrder[8] = {4,7,2,1,5,3,8,6};
	BinaryTreeNode* root = rebulidBinaryTree(preOrder, inOrder, 8);

	mirrorRecursively(root);
	printInOrder(root);
	return 0;
}
```
