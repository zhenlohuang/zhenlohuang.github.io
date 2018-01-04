---
layout: post
title: 'Python核心编程 练习 第五章'
date: 2010-5-19
wordpress_id: 425
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
下面的代码是这两天看Python核心编程的时候做的部分课后练习

``` python 
#!/usr/bin/env python
# -*- coding: utf-8 -*-
#5-2 运算符
#(a) 写一个函数，计算并返回两个数的乘积
#(b) 写一段代码调用这个函数，并显示它的结果
def mult(x,y) :
    '返回两个数x和y的乘积'
    return int(x) * int(y)  #这里int()函数还是要的
a = raw_input('输入数a:')
b = raw_input('输入数b:')
print 'a * b = ', mult(a,b)
```

``` python 
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#5-3 标准类型运算符. 写一段脚本，输入一个测验成绩，根据下面的标准，输出他的评分成绩（A-F）。
#A: 90–100 B: 80–89 C: 70–79 D: 60 F: <60
while True :
    score = raw_input('输入分数：')
    score = int(score)         #将score类型由string转换为int
    if score > 100 or score < 0 :
        print '输入分数无效，程序退出'
        break
    elif score >= 90 :
        print '%d is rank A' % score
    elif score >= 80 :
        print '%d is rank B' % score
    elif score >= 70 :
        print '%d is rank C' % score
    elif score >= 60:
        print '%d is rank D' % score
    else :
        print '%d is rank F' % score
```

``` python 	
#!/usr/bin/env python
#-*- coding: utf-8 -*-
#5-4 取余。判断给定年份是否是闰年。使用下面的公式：
#一个闰年就是指它可以被4 整除，但不能被100 整除， 或者它既可以被4 又可以被100 整
#除。比如 1992，1996 和2000 年是闰年，但1967 和1900 则不是闰年。下一个是闰年的整世
#纪是 2400 年。
def isLeap(year) :
    year = int(year)
    if (year % 4 == 0 and year % 100 != 0) or year % 400 == 0 :
        return True
    else :
        return False

year = raw_input('输入一个年份：')
if (isLeap(year)) :
print year, '是闰年'
else :
print year, '不是闰年'
```

``` python 
#!/usr/bin/env python
#-*- coding:utf-8 -*-
#5-5 取余。取一个任意小于1 美元的金额，然后计算可以换成最少多少枚硬币。硬币有1
#美分，5 美分，10 美分，25 美分四种。1 美元等于100 美分。举例来说，0.76 美元换算结果
#应该是 3 枚25 美分，1 枚1 美分。类似76 枚1 美分，2 枚25 美分+2 枚10 美分+1 枚5 美分+1
#枚1 美分这样的结果都是不符合要求的。
money = raw_input('输入任意小于1 美元的金额:')
print money,'美元换算结果'
money = float(money)
money *= 100
money = int(money)
cent25 = money / 25
money %= 25
cent10 = money / 10
money %= 10
cent5 = money / 5
money %= 5
cent1 = money
if cent25 :
    print '25美分*',cent25
if cent10 :
    print '10美分*',cent10
if cent5 :
    print '5美分*',cent5
if cent1 :
    print '1美分*',cent1
```

``` python 
#!/usr/bin/env python
#-*- coding:utf-8 -*-
#5-6 算术。写一个计算器程序 你的代码可以接受这样的表达式，两个操作数加一个运算符：
#N1 运算符 N2. 其中 N1 和 N2 为整数或浮点数，运算符可以是+, -, *, /, %, ** 分别表示
#加法，减法， 乘法， 整数除，取余和幂运算。计算这个表达式的结果，然后显示出来。提示：
#可以使用字符串方法 split(),但不可以使用内建函数 eval().
def operator(str) :
    if str.find('+') >= 0:
        return '+'
    elif str.find('-') >= 0:
        return '-'
    elif str.find('*') >= 0:
        return '*'
    elif str.find('/') >= 0:
        return '/'
    elif str.find('%') >= 0:
        return '%'
    else :
        return '**'
calStr = raw_input('输入计算表达式,如a+b形式：')
op = operator(calStr)
num = calStr.split(op)
print num
res = 0.0
if op == '+':
    res = float(num[0]) + float(num[1])
elif op == '-':
    res = float(num[0]) - float(num[1])
elif op == '*':
    res = float(num[0]) * float(num[1])
elif op == '/':
    res = float(num[0]) / float(num[1])
elif op == '%':
    res = int(num[0]) % int(num[1])
elif op == '**':
    res = float(num[0]) ** float(num[1])
print 'result is %f' % str(res)
```

``` python 
#!/usr/bin/env python
#-*- coding:utf-8 -*-
#5-10 转换。写一对函数来进行华氏度到摄氏度的转换。转换公式为C = (F - 32) * (5 / 9)
#应该在这个练习中使用真正的除法， 否则你会得到不正确的结果。
F = float(raw_input('输入温度：'))
C = (F-32)*(5/9.0)
print '华氏度到摄氏度的转换后，结果为：',C
```
 

``` python 
#!/usr/bin/env python
#-*- coding:utf-8 -*-
#5-11 取余。
#(a) 使用循环和算术运算，求出 0－20 之间的所有偶数
#(b) 同上，不过这次输出所有的奇数
#(c) 综合 (a) 和 (b)， 请问辨别奇数和偶数的最简单的方法是什么？
#(d) 使用(c)的成果，写一个函数，检测一个整数能否被另一个整数整除。 先要求用户输
#入两个数，然后你的函数判断两者是否有整除关系，根据判断结果分别返回 True 和 False;
#(a)
print '0－20 之间的所有偶数:'
i = 0
while i <= 20 :
    if (i % 2 == 0) :
        print i,' '
    i += 1
print
#(b)
print '0－20 之间的所有奇数:'
i = 0
while i <= 20 :
    if (i % 2 == 1) :
        print i,' '
    i += 1
print
#(d)
a = int(raw_input('输入整数a:'))
b = int(raw_input('输入整数b:'))
if (a % b == 0):
    print 'a能被b整除'
else:
    print 'a不能b整除'
```

``` python 
#!/usr/bin/env python
#-*- coding:utf-8 -*-
#5–15. 最大公约数和最小公倍数。请计算两个整数的最大公约数和最小公倍数。
def GCD(a, b):
    '求a和b的最大公约数'
    if b == 0 :
        return a
    else :
        return GCD(b, a % b)
def LCM(a, b):
    '求a和b的最小公倍数'
    if (a * b == 0):
        return 0
    else :
        return a * b / GCD(a,b)

x = int(raw_input('输入x:'))
y = int(raw_input('输入y:'))
print 'GCD:',GCD(x, y)
print 'LCM:',LCM(x, y)
```
