---
layout: post
title: 'Sub 与 Function 在VB程序设计中的区别'
date: 2011-7-17
wordpress_id: 509
categories: [Programming, VB]
tags: [VB]
keywords: "VB"
description: 
comments: true
---

Private Sub 与 Function 在VB程序设计中的区别    
function是函数，sub是子程序，都可以传递参数，但函数有返回值，子程序没有    
function 可以用自身名字返回一个值，sub 需定义别的变量，用传址方式传回值。    

Sub 过程与Function 过程的区别：     
1． Sub 过程定义时无需定义返回值类型，而Function 过程一般需要用“As 数据类型” 定义函数返回值类型。     
2． Sub 过程中没有对过程名赋值的语句，而Function 过程中一定有对函数名赋值的语句。     
3． 调用过程：调用 Sub 过程与 Function 过程不同。调用 Sub 过程的是一个独立的语句，而调用函数过程只是表达式的一部分。Sub 过程还有一点与函数不一样，它不会用名字返回一个值。但是，与 Function过程一样，Sub 过程也可以修改传递给它们的任何变量的值。     
4． 调用 Sub 过程有两种方法：     
以下两个语句都调用了名为 MyProc 的 Sub 过程。     
Call MyProc (FirstArgument, SecondArgument)     
MyProc FirstArgument, SecondArgument     
注意当使用 Call 语法时，参数必须在括号内。若省略 Call 关键字，则也必须省略参数两边的括号。    
