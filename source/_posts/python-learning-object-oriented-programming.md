---
layout: post
title: 'Python学习  面向对象编程'
date: 2010-8-5
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
这里自行定义了一个MyTime的类，继承于系统类object。在Python里面默认的情况的下都要继承于这个类。类里面对__init__,__str__，__add__，__iadd__函数进行了重载。其实严格上讲不能叫重载，因为Python不支持重载，确切说应该叫覆盖。里面我还企图对__init__进行再次重载，显然不允许的，放在那边做个比较。

``` python 
#!/usr/bin/env python
class MyTime(object) :
    'MyTime - operate hours, minutes and seconds'
    def __init__(self, h, m, s) :
        'MyTime - Constructor'
        self.hour = h;
        self.min = m;
        self.sec = s;
        
    #def __init__(self, seconds) :
    #    'MyTime - Constructor'
    #    self.hour = seconds / 3600
    #    seconds = seconds % 3600
    #    self.min = seconds /60
    #    self.sec = seconds % 60
        
    def __str__(self) :    #显示函数重载
        'MyTime - string representation'
        return '%d : %d : %d' % (self.hour, self.min, self.sec)
    __repr__ = __str__
    def __add__(self, time) :   #加法重载
        'MyTime - overloading the addition operator'
        h = m = s = 0
        t = self.sec + time.sec
        s = t % 60
        m += t /60
        t = self.min + time.min + m
        m = t % 60
        h = t / 60
        t = self.hour + time.hour + h
        h = t % 24
        return self.__class__(h, m, s)
                       
    def __iadd__(self, time) :   #自增重载
        'MyTime - overloading the in-place addition operator'
        t = self.sec + time.sec
        self.sec = t % 60
        self.min += t / 60
        t = self.min + time.min
        self.min = t % 60
        self.hour += t /60
        t = self.hour + time.hour
        self.hour = t % 24
        return self
        
#测试部分
t1 = MyTime(12,34,57)
t2 = MyTime(8,6,56)
print 't1 >> ', t1
print 't2 >> ', t2
print 't1 + t2 >> ', t1 + t2
t1 += t2
print 't1 += t2 >> ', t1 
#测试结果
#t1 >>  12 : 34 : 57
#t2 >>  8 : 6 : 56
#t1 + t2 >>  20 : 41 : 53
#t1 += t2 >>  20 : 41 : 53
```


