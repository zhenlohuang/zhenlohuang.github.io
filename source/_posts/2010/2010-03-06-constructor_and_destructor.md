---
layout: post
title: '构造函数与析构函数研究'
date: 2010-3-6
wordpress_id: 403
permalink: /archives/constructor_and_destructor.html
categories: [Programming]
tags: [C++]
keywords: "C++"
description: 
comments: true
---
首先先贴代码

``` cpp
#include <iostream>
using namespace std;
class A
{
public:
 A() { cout << "class A is constructed" << endl;}
 ~A() { cout << "class A is destroyed" << endl; }
//method
 void fa() { cout << "class A fa method" << endl;}
 void fb() { cout << "class A fb method" << endl;}
};
class B : public A
{
public:
 B() { cout << "class B is constructed" << endl;}
 ~B() { cout << "class B is destroyed" << endl; }
 //method
 void fa() { cout << "class B fa method" << endl;}
 void fb() { cout << "class B fb method" << endl;}
};
int main()
{
 A *pa = new A;
 B *pb = new B;
 cout << endl;
 pa->fa();
 pa->fb();
 cout << endl;
 pb->fa();
 pb->fb();
 cout << endl;
 delete pa;
 delete pb;
 cout << endl;
 return 0;
}
```

```
运行结果如下：
class A is constructed
class A is constructed
class B is constructed
class A fa method
class A fb method
class B fa method
class B fb method
class A is destroyed
class B is destroyed
class A is destroyed
```

细心的娃肯定能发下些问题了，在子类的构造函数中先构造父类然后在构造子类，析构时先析构子类然后再析构父累





PS：这个题上次去Foxit有考，刚开始我是这么想的，后来居然给改了，郁闷阿...........


