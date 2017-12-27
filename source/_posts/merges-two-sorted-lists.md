---
layout: post
title: '【IT笔试面试题整理】合并两个排序的链表'
date: 2012-9-23
wordpress_id: 3403
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】合并两个递增排序的链表，合并这两个链表使得新链表的节点也是按递增的顺序拍列的。

【试题来源】未知

【参考代码】

``` cpp 
#include <iostream>
using namespace std;

struct ListNode {
	int value;
	ListNode* next;
};

ListNode* createList(int* data, int n) {

	ListNode* headNode = NULL;
	ListNode* lastNode = NULL;

	for(int i = 0; i < n; i++) {
		ListNode* node = new ListNode();
		node->value = data[i];
		node->next = NULL;
		if(i == 0) {
			headNode = node;
		} else {
			lastNode->next = node;
		}
		lastNode = node;
	}

	return headNode;
}

void printList(ListNode* headNode) {

	if(headNode == NULL) {
		return ;
	}
	ListNode* node = headNode;
	while(node != NULL) {
		cout << node->value << " ";
		node = node->next;
	}
}

ListNode* mergeSortedList(ListNode* headNodeA, ListNode* headNodeB) {

	if(headNodeA == NULL) {
		return headNodeB;
	} else if(headNodeB == NULL) {
		return headNodeA;
	}

	ListNode* nodeIndexA = headNodeA;
	ListNode* nodeIndexB = headNodeB;
	ListNode* headNode = NULL;
	ListNode* node = NULL;

	if(headNodeA->value < headNodeB->value) {
		headNode = node = headNodeA;
		nodeIndexA = nodeIndexA->next;
	} else {
		headNode = node = headNodeB;
		nodeIndexB = nodeIndexB->next;
	}

	while(nodeIndexA != NULL && nodeIndexB != NULL) {

		if(nodeIndexA->value < nodeIndexB->value) {
			node->next = nodeIndexA;
			node = node->next;
			nodeIndexA = nodeIndexA->next;
		} else {
			node->next = nodeIndexB;
			node = node->next;
			nodeIndexB = nodeIndexB->next;
		}
	}

	if(nodeIndexA != NULL) {
		node->next = nodeIndexA;
	} else if(nodeIndexB != NULL) {
		node->next = nodeIndexB;
	}

	return headNode;
}

int main() {

	int data1[] = {1, 3, 5, 7, 9, 11, 13};
	int n1 = 7;
	ListNode* headNodeA = createList(data1, n1);
	int data2[] = {2, 4, 6, 8, 10, 12, 14};
	int n2 = 7;
	ListNode* headNodeB = createList(data2, n2);

	ListNode* headNode = mergeSortedList(headNodeA, headNodeB);
	printList(headNode);

	return 0;
}
```
