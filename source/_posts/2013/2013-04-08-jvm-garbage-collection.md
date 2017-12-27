---
layout: post
title: 'JVM学习笔记 垃圾回收'
date: 2013-4-8
wordpress_id: 3942
permalink: /archives/jvm-garbage-collection.html
categories: [Programming]
tags: [JVM]
keywords: "JVM"
description: 
comments: true
---
# 判断对象是否存活
**1）引用计数算法（Reference Counting）**    
    给对象中添加一个引用计数器，每当一个地方引用它时计数器+1,；当引用失效时，计数器-1；任何时刻计数器为0的对象就是不可能再被使用。    
    举例：Python，Flashplayer，COM    
    缺陷：无法解决对象之间的循环引用问题。    

**2）根搜索算法**
    以GC Roots作为起点，从这些节点开始向下搜索搜索所走过的路径称为引用链（Reference Chain），当一个对象到GC Roots没有和任何引用链相连（也可以理解为图论中的该节点不可达），则该对象是不可用的。    
    Java中，可以作为GC Roots的对象包括：    
    ①Stack中（Stack Frame的本地变量表）中的引用对象。    
    ②Method Area中的静态属性引用对象和常量引用对象。    
    ③Native Method Stack中JNI的引用对象。    

# 引用
**1）强引用（Strong Reference）**：类似“Object obj=new Object()”这类的引用，只要强引用存在GC就不会收回被引用的对象。    
**2）软引用（Soft Reference）**：对于软引用的对象，在系统将要发生内存溢出之前，将会把这些对象列进回收的范围，进行第二次回收。如果还没有足够的内存，就会抛出内存溢出异常。    
**3）弱引用（Weak Reference）**：被弱引用关联的对象只能保留到下一次垃圾回收发生之前。    
**4）虚引用（Phantom Reference）**：被虚引用的对象，完全不会影响其生存时间构成，也无法通过虚引用取得一个实例。虚引用的唯一目的就是在对象被GC回收时可以收到一个通知。    

# 垃圾收集算法
**1）标记-清除算法**
    首先标记出所有需要回收的对象，在标记完成后统一回收掉所有被标记的对象。    
    优点：简单    
    缺点：效率低下，且容易产生不连续的区域    
**2）复制算法**
    将内存空间分为Eden和Survivor两块区域，当一块区域内存用完时，将存活的对象复制到另一块区域，然后把之前的区域清理掉。    
    优点：简单高效，不会产生不连续的区域    
    缺点：内存利用率只有50%    
**3）标记-整理算法**
    相比标记-清除算法，在标记之后并不马上进行回收，而是将存活的对象向一端移动，然后清理掉端外内存。    
**4）分代收集算法**
    将Java Heap分为新生代（Younger Generation）和老年代（Tenured Generation），根据各个年代的特点采用最适合的收集算法。    