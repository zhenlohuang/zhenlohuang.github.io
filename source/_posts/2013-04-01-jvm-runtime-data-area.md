---
layout: post
title: '【JVM学习笔记】运行时数据区域'
date: 2013-4-1
categories: [Programming, Java]
tags: [JVM]
keywords: "JVM"
description: 
comments: true
---
JVM运行时数据区域示意图如下所示:

![image](/images/legacy/2013/04/JVM_Runtime_Data_Area.png)

共享数据区域：Method Area、Heap    
私有数据区域：VM Stack、Native Method Stack、Program Counter Register    

**1）方法区（Method Area）**    
    用于存储已被虚拟机加载的class信息、常量、静态变量、即时编译后的代码等数据。    
    Exception：OutOfMemoryError    

**2）Java虚拟机栈（JVM Stack）**    
    每个方法被执行时都会创建一个Stack Frame用于存储局部变量表、操作栈、动态链接、方法出口等信息。每个方法被调用直至执行完成，就对应着一个Stack Frame在虚拟机栈中从入栈到出栈的过程。    
    Exception：StackOverflowError、OutOfMemoryError    

**3）本地方法区（Native Method）**    
    为使用Native方法服务的。    
    Exception：StackOverflowError、OutOfMemoryError    

**4）Java堆（Java Heap）**    
    JVM中最大的一块区域。Java Heap是被所有线程所共享，在JVM启动时创建。    
    在JVM规范中的描述如下：The heap is the runtime data area from which memory for all class instances and arrays is allocated.    

**5）程序计数器（Program Counter Register）**    
    用于指示当前线程所执行的字节码行号指示器。    
    Exception：None    

**6）运行时常量池（Runtime Constant Pool）**    
    是方法区的一部分。用于存放编译时生成的各种字面变量和常用符号    
    Exception：OutOfMemoryError    

