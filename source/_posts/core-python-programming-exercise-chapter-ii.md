---
layout: post
title: 'Python核心编程 练习 第二章'
date: 2010-5-3
wordpress_id: 420
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---

``` python
#2-2启动交互解释器，使用Python对两个数值(任意类型)进行加、减、乘、除运算。然后使用取余运算符来
#得到两个数相除的余数， 最后使用乘方运算符求A 数的B 次方。
#!/usr/bin/python 
a = 8
b = 5
print 'a + b = %d' % (a + b)
print 'a - b = %d' % (a - b)
print 'a * b = %d' % (a * b)
print 'a / b = %d' % (a / b)
print 'a mod b = %d' % (a % b)
print 'a ^ b = %d' % (a ** b) 
```

``` python
#2–4. 使用raw_input()函数得到用户输入
#(a) 创建一段脚本使用 raw_input() 内建函数从用户输入得到一个字符串，然后显示这个
#用户刚刚键入的字符串。
#(b) 添加一段类似的代码，不过这次输入的是数值。将输入数据转换为一个数值对象，（使
#用int()或其它数值转换函数） 并将这个值显示给用户看。（注意，如果你用的是早于1.5 的版
#本，你需要使用string.ato*() 函数执行这种转换）
#!/usr/bin/python 
str = raw_input('Input a string:')
print str
num = raw_input('input a number:')
print int(num) 
```

``` python
#2–5. 循环和数字
#分别使用while 和for 创建一个循环:
#(a) 写一个while 循环，输出整数从0 到10。（要确保是从0 到10， 而不是从0 到9 或从1 到10）
#(b) 做同 (a) 一样的事， 不过这次使用 range() 内建函数。
#!/usr/bin/python
i = 0
while i <= 10 :
    print i
    i += 1
print 
for i in  range(10) :
    print i 
```

``` python
#2–6. 条件判断 判断一个数是正数，还是负数， 或者等于0. 开始先用固定的数值，然后修改你的代码支持用户输入数值再进行判断。
#!/usr/bin/python
num = raw_input('Input a number:')
num = int(num)
if num > 0 :
    print 'The Number is a Positive Number'
elif num < 0:
    print 'The Number is a Negative Number'
else :
    print 'The Number is Zero'
```

``` python
#2–7.
#循环和字串 从用户那里接受一个字符串输入，然后逐字符显示该字符串。先用while 循环实现，然后再用 for 循环实现。
#!/usr/bin/python
str = raw_input('Input a string:')
i = 0;
while i < len(str) :
    print str[i]
    i += 1

for c in str :
    print c
```

``` python
#2–8. 循环和运算符 创建一个包含五个固定数值的列表或元组，输出他们的和。然后修
#改你的代码为接受用户输入数值。 分别使用while 和for 循环实现。
#!/usr/bin/python
print 'Input five numbers:'
i = 0
sum = 0
while i < 5 :
    num = raw_input()
    sum += int(num)
    i += 1

print 'The Sum is: %d' % sum
```

``` python
#2–9.
#循环和运算符 创建一个包含五个固定数值的列表或元组，输出他们的平均值。本练习的难
#点之一是通过除法得到平均值。 你会发现整数除会截去小数，因此你必须使用浮点除以得到更
#精确的结果。 float()内建函数可以帮助你实现这一功能。
#!/usr/bin/python
print 'Input five numbers:'
i = 0
sum = 0.0
while i < 5 :
    num = raw_input()
    sum += float(num)
    i += 1
avg = sum / 5
print 'The Average is: %f' % avg
```

``` python
#2–10.
#带循环和条件判断的用户输入 使用raw_input()函数来提示用户输入一个1 和100 之间的
#数，如果用户输入的数满足这个条件，显示成功并退出。否则显示一个错误信息然后再次提示
#用户输入数值，直到满足条件为止。
#!/usr/bin/python
while 1 :
    num = raw_input('Input a number(1-100):')
    num = int(num)
    if (num >= 0) and (num <= 100) :
        print 'Inut Success'
        break
    else :
        print 'Input Again'
```

``` python
#2–11.
#带文本菜单的程序 写一个带文本菜单的程序，菜单项如下（1）取五个数的和 (2) 取五个
#数的平均值....（X）退出。由用户做一个选择，然后执行相应的功能。当用户选择退出时程序
#结束。这个程序的有用之处在于用户在功能之间切换不需要一遍一遍的重新启动你的脚本
#!/usr/bin/python
i = 0
sum = 0
print 'Input five numbers:'
while i < 5:
    num = raw_input()
    sum += float(num)
    i += 1

while 1:
    print '1.Sum'
    print '2.Average'
    print 'x.Exit'
    select = raw_input('Input your select:')
    if select == '1' :
        print 'The Sum is %d' % sum
    elif select == '2' :
        avg = sum / 5
        print 'The Average is %f' % avg
    else :
        break;
```
