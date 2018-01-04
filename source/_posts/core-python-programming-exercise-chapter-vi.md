---
layout: post
title: 'Python核心编程 练习 第六章'
date: 2010-5-31
wordpress_id: 439
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---

``` python 
#!/usr/bin/env python
#-*- coding:utf-8 -*-
#6&ndash;2. 字符串标识符.修改例6-1 的idcheck.py 脚本,使之可以检测长度为一的标识符,并且
#可以识别Python 关键字,对后一个要求,你可以使用keyword 模块(特别是keyword.kelist)来帮你.
import string
import keyword
alphas = string.letters
nums = string.digits
print 'The Identifier Checker'
myString = raw_input('Input the identifier to check:')
if myString in keyword.kwlist :
	print 'Invalid: It is a keyword.'
else:
	if myString[0] not in alphas + '_' :
		print 'Invalid: First symbol must be alphabetic or underline.'
	else:
		for c in myString[1:] :
			if c not in alphas + nums :
				print 'Invalid: Symbols must be alphabetic or numbers.'
		print 'The identifier is ok.' 
```

``` python 
#!/usr/bin/env python
#-*- coding:utf-8 -*-
#6&ndash;3. 排序
#(a) 输入一串数字,从大到小排列之.
#(b) 跟a 一样,不过要用字典序从大到小排列之.
#(a)
list1 = []
while True:
	n = int(raw_input('输入一些数字以0结束：'))
	if n == 0:
		break
	list1.append(n)
list1.sort()
print list1
#(b)
list2 = []
while True:
	n = raw_input('输入一些数字以0结束：')
	if n == '0':
		break
	list2.append(n)
list2.sort()
print list2 
```

``` python 
#!/usr/bin/env python
# -*- coding: utf-8 -*-
#6&ndash;6. 字符串.创建一个string.strip()的替代函数:接受一个字符串,去掉它前面和后面的空格
str = raw_input('Input a string your want to operate:')
i = 0
j = len(str) - 1
while str[i] == ' ' :
		i += 1
		
while str[j] == ' ' :		
		j -= 1
				
str = str[i:j+1]
print str 
```

``` python 
#!/usr/bin/env python
# -*- coding: utf-8 -*-
#6&ndash;9. 转换.为练习5-13 写一个姊妹函数, 接受分钟数, 返回小时数和分钟数. 总时间不
#变,并且要求小时数尽可能大.
n = int(raw_input('输入分钟数：'))
hour = n / 60
min = n % 60
print '%d : %d' % (hour, min)
```

``` python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
#6&ndash;10.字符串.写一个函数,返回一个跟输入字符串相似的字符串,要求字符串的大小写反转.
#比如,输入"Mr.Ed",应该返回"mR.eD"作为输出.
def mySwapcase(myString) :
	return myString.swapcase()
	
str = raw_input('Input a string your want to operate:')
print mySwapcase(str)
```

``` python 
#!/usr/bin/env python
# -*- coding: utf-8 -*-
#6&ndash;12.字符串
#(a)创建一个名字为findchr()的函数,函数声明如下:def findchr(string, char)
#findchr()要在字符串string 中查找字符char,找到就返回该值的索引,否则返回-1.不能用
#string.*find()或者string.*index()函数和方法
#(b)创建另一个叫rfindchr()的函数,查找字符char 最后一次出现的位置.它跟findchr()工作
#类似,不过它是从字符串的最后开始向前查找的.
#(c)创建第三个函数,名字叫subchr(),声明如下:def subchr(string, origchar, newchar)
#subchr()跟findchr()类似,不同的是,如果找到匹配的字符就用新的字符替换原先字符.返回
#修改后的字符串.
def findchr(string, char) :
	i = 0;
	while i < len(string) :
		if string[i] == char :
			return i
		i += 1
	return -1
	
def rfindchr(string, char) :
	i = len(string) - 1
	while i >= 0 :
		if string[i] == char :
			return i
		i -= 1
	return -1
	
def subchr(myString, origchar, newchar) :
	'将string中的origchar字符替换成newchar字符'
	import string
	string.replace(myString,origchar,newchar)

#Test
myString = 'abcdefcg'
print findchr(myString, 'c')
print findchr(myString, 'c')
subchr(myString, 'c', 'X')
print myString 
``` 