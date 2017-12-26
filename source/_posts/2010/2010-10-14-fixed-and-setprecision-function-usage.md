---
layout: post
title: '【资料整理】fixed()和setprecision()函数的用法'
date: 2010-10-14
wordpress_id: 477
permalink: /archives/fixed-and-setprecision-function-usage.html
categories: [Programming]
tags: [C++]
keywords: "C++"
description: 
comments: true
---
使用```setprecision(n)```可控制输出流显示浮点数的数字个数。C++默认的流输出数值有效位是6。
如果```setprecision(n)```与```setiosflags(ios::fixed)```合用，可以控制小数点右边的数字个数。```setiosflags(ios::fixed)```是用定点方式表示实数。 
如果与```setiosnags(ios::scientific)```合用， 可以控制指数表示法的小数位数。```setiosflags(ios::scientific```)是用指数方式表示实数。
例如，下面的代码分别用浮点、定点和指数方式表示一个实数:

``` cpp 
#include <iostream.h>
#include <iomanip.h> //要用到格式控制符
 
   void main()
   {
     double amount = 22.0/7;
     cout <<amount <<endl;
     cout <<setprecision(0) <<amount <<endl
      <<setprecision(1) <<amount <<endl
      <<setprecision(2) <<amount <<endl
      <<setprecision(3) <<amount <<endl
      <<setprecision(4) <<amount <<endl;
 
     cout <<setiosflags(ios::fixed);
     cout <<setprecision(8) <<amount <<endl;
     cout <<setiosflags(ios::scientific) <<amount <<endl;
 
 
     cout <<setprecision(6); //重新设置成原默认设置
   }
```

运行结果为：

```
3.14286
3
3
3.1
3.14
3.143
3.14285714
3.14285714e+00
```

在用浮点表示的输出中，```setprecision(n)```表示有效位数。
第1行输出数值之前没有设置有效位数，所以用流的有效位数默认设置值6：
第2个输出设置了有效位数0，C++最小的有效位数为1，所以作为有效位数设置为1来看待：
第3～6行输出按设置的有效位数输出。在用定点表示的输出中，```setprecision(n)```表示小数位数。
第7行输出是与```setiosflags(ios::fixed)```合用。所以```setprecision(8)```设置的是小数点后面的位数，而非全部数字个数。 在用指数形式输出时，```setprecision(n)```表示小数位数。
第8行输出用```setiosflags(ios::scientific)```来表示指数表示的输出形式。其有效位数沿用上次的设置值8

资料来源：   
<http://zhidao.baidu.com/question/31863763.html>
<http://topic.csdn.net/u/20101014/10/0df6e43c-01a1-4354-b282-94d86e3f908d.html>