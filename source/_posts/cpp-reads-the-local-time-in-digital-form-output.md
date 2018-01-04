---
layout: post
title: 'C++读取本地时间，用数码形式输出'
date: 2010-3-8
wordpress_id: 405
categories: [Programming, C++]
tags: [C++]
keywords: "C++"
description: 
comments: true
---
这个要是用Qt或者MFC等图形库估计很容易就实现了，可是用字符界面还是很麻烦的，具体看代码

``` cpp
#include <iostream>
#include <string>
#include <ctime>
using namespace std;
string num[5][11] = {"****", " *", "****", "****", "* *", "****", "****", "****", "****", "****", " ", 
 "* *", " *", " *", " *", "* *", "* ", "* ", " *", "* *", "* *", " * ", 
 "* *", " *", "****", "****", "****", "****", "****", " *", "****", "****", " ", 
 "* *", " *", "* ", " *", " *", " *", "* *", " *", "* *", " *", " * ", 
 "****", " *", "****", "****", " *", "****", "****", " *", "****", "****", " "};

int main()
{
	int i,j;
	/*for(i= 0;i <a href="http://grandruby.co.uk/tips-on-choosing-the-best-poker-rooms">online casinos</a>  < 10; i ) {
		for(j = 0; j < 10; j ) {
			cout << num[i][j] << " ";
		}
		cout << endl;
	}
	cout << endl << endl;*/

	time_t t= time(0);
	tm *curtime = localtime(&t);

	int hour = curtime->tm_hour;
	int min = curtime->tm_min;
	int sec = curtime->tm_sec;
	cout << hour << ":" << min << ":" << sec << endl;
	cout << endl;

	int timeArray[8];
	timeArray[0] = hour / 10;
	timeArray[1] = hour % 10;
	timeArray[2] = 10;
	timeArray[3] = min / 10;
	timeArray[4] = min % 10;
	timeArray[5] = 10;
	timeArray[6] = sec / 10;
	timeArray[7] = sec % 10;

	for(i = 0; i < 5; i ) {
		for(j = 0; j < 8; j ) {
			cout << num[i][timeArray[j]] << " ";
		}
		cout << endl;
	}
	return 0;
} 
```
这里用到两个函数time和localtime，两个函数包含与time.h里面
time_t time( time_t *time );
功能： 函数返回当前时间，如果发生错误返回零。如果给定参数time ，那么当前时间存储到参数time中。
struct tm *gmtime( const time_t *time );
功能：函数返回给定的统一世界时间（通常是格林威治时间），如果系统不支持统一世界时间系统返回NULL。

PS：C语言果然博大精深....
