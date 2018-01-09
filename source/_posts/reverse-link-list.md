---
layout: post
title: '【IT笔试面试题整理】反转链表'
date: 2012-9-20
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点

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

ListNode* reverseList(ListNode* headNode) {

	if(headNode == NULL) {
		return NULL;
	}

	ListNode* curNode = headNode;
	ListNode* preNode = NULL;
	ListNode* nextNode = NULL;

	while(curNode != NULL) {

		nextNode = curNode->next;
		curNode->next = preNode;
		preNode = curNode;
		curNode = nextNode;

	}
	return preNode;
}

int main() {
	int data[] = {1, 2, 3, 4, 5, 6, 7};
	int n = 7;
	ListNode* headNode = createList(data, n);
	ListNode* reverseNode = reverseList(headNode);
	printList(reverseNode);
	return 0;
}
```
