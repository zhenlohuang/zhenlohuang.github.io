---
layout: post
title: 'C++类定义空间占用研究'
date: 2012-9-4
wordpress_id: 3291
permalink: /archives/c-class-definition-space-research.html
categories: [Programming]
tags: [C++]
keywords: "C++"
description: 
comments: true
---
最近准备重温C++，正好看到有关类和占用空间的问题，于是整理了下。先上代码

``` cpp 
#include <iostream>
using namespace std;

//空类型，没有任何成员
class ClassA {

};

//添加构造函数和析构函数
class ClassB {

public:
	ClassB() {};
	~ClassB() {};

};

//仅包含一个虚构函数
class ClassC {
	virtual void fun() {};
};

int main() {

	ClassA A;
	cout << "ClassA:" << sizeof(A) << endl;

	ClassB B;
	cout << "ClassB:" << sizeof(B) << endl;

	ClassC C;
	cout << "ClassC:" << sizeof(C) << endl;

	return 0;
}
```

结果：

```
ClassA:1
ClassB:1
ClassC:4
```
【分析】
1）ClassA里面没有定义任何变量和函数，本应该是0的，但是声明变量需要占用一个字节，因此是1。    
2）ClassB里面仅仅定义了构造函数和析构函数。调用构造函数和析构函数只需要知道地址即可，而这些函数的地址只与类型相关，而与实例无关，编译器不会因为这两个函数在实例中添加任何信息。    
3）ClassC中仅有一个虚函数。C++编译器一旦发现类型中有虚函数，就会为该类型生成虚函数表，并在该类型的每个实例中添加一个指向虚函数表的指针。
    
PS：编译器GCC 4.6.2    
