---
layout: post
title: '【IT笔试面试题整理】在O(1)时间删除链表节点'
date: 2012-9-11
wordpress_id: 3395
categories: [Interview]
tags: []
keywords: ""
description: 
comments: true
---
【试题描述】给定单向链表的头指针和一个节点指针，定义一个函数在O(1)时间删除该节点。

【试题来源】 未知

【试题分析】按照常规删除链表节点的方法没有办法在O(1)复杂度内完成，因此需要转换思路，将后面一个节点的内容复制过来，然后删除其后面的那个节点，以达到删除节点的目的。

【参考代码】

``` cpp 
struct ListNode
{
	int value;
	ListNode* next;
};

void deleteLinkNode(ListNode** pHeadNode, ListNode* pDeleteNode) {

	if(pHeadNode == NULL || pDeleteNode == NULL) {
		return ;
	}

	//删除节点在头部
	if(pDeleteNode == *pHeadNode) {
		delete pDeleteNode;
		pDeleteNode = NULL;
		*pHeadNode = NULL;
	//删除节点在末尾
	} else if(pDeleteNode->next == NULL) {
		ListNode* pNode = *pHeadNode;
		while(pNode->next != pDeleteNode) {
			pNode = pNode->next;
		}
		pNode->next = NULL;
		delete pDeleteNode;
		pDeleteNode = NULL;
	} else {
		ListNode* pNode = pDeleteNode->next;

		pDeleteNode->value = pNode->value;
		pDeleteNode->next = pNode->next;

		delete pNode;
		pNode = NULL;
	}
}
```
