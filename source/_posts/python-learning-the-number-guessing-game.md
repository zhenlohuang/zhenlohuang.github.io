---
layout: post
title: 'Python学习  猜数字游戏'
date: 2010-8-5
wordpress_id: 461
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
这里的程序并不复杂，下面主要对随机数据的生成部分进行分析

``` python 
def randNumGen(self) :
        numStr = '0123456789'
        randNum = ''
        for i in range(4) :
            n = choice(numStr)
            randNum += n
            numStr = numStr.replace(n, '')
        self.sysNum = randNum
```

这里由于数字是不重复的，所以可以先定义好一个0-9的数字串，然后用choice函数随机选出一个，之后删除这个数字，再次choice，直到选出4位数字为止。

下面是完整的代码：

``` python 
#!/usr/bin/env python
#猜数字游戏
from string import digits
from random import randint, choice
class GuessNumber(object) :
    #构造器
    def __init__(self) :
        self.count = 0      #用于统计猜测次数
        self.usrNums = []    #用户所猜数字
        self.sysNum = ''    #系统生成数字
        self.results = []   #猜测结果
        self.isWinner = False
    #打印规则
    def printRules(self) :
        print """由系统随机生成不重复的四位数字，用户猜，之后系统进行提示。
                 A代表数字正确位置正确，B代表数字正确位置错误。
                 如正确答案为 5234，而猜的人猜 5346，则是 1A2B，其中有一个5的位置对了
                 ，记为1A;3和4这两个数字对了，而位置没对，因此记为 2B，合起来就是 1A2B。
　　              接着猜的人再根据出题者的几A几B继续猜，直到猜中（即 4A0B）为止"""
    #随机数字生成
    def randNumGen(self) :
        numStr = '0123456789'
        randNum = ''
        for i in range(4) :
            n = choice(numStr)
            randNum += n
            numStr = numStr.replace(n, '')
        self.sysNum = randNum
        #print 'Debug >> SysNum::', self.sysNum
    #数字输入
    def numInput(self) :
        self.count += 1
        while True:
            num = raw_input('输入您想要猜的数字:')
            print 'Debug >> num::',num
            if len(num) < 4 :
                print '***错误： 位数小于四位，请重新输入'
                continue
            elif len(num) > 4:
                print '***错误: 位数大于四位，请重新输入'
                continue
            if not self.isAllNumber(num) :
                print '***错误: 您输入了非数字字符，请重新输入'
                continue
            elif self.hasSameDigit(num) :
                print '***错误: 您输入了含有相同数字的数字串，请重新输入'
                continue
            else :
                self.usrNums.append(num)
                return num
    #判断是否都是数字
    def isAllNumber(self, num) :
        for ch in num :
            if ch not in digits :
                return False
        return True
        
    #判断是否有相同的字符
    def hasSameDigit(self, num) :
        for i in range(len(num)) :
            pos = num[i + 1 :].find(num[i])
            if pos >= 0 :
                return True
        return False
    #判断
    def numJudge(self, sysNum, usrNum) :
        countA = 0
        countB = 0
        for i in range(4) :
            if usrNum[i] in sysNum :
                if i == sysNum.find(usrNum[i]) :
                    countA += 1
                else :
                    countB += 1
        result = '%dA%dB' % (countA, countB)
        self.results.append(result)
        if countA == 4 :
            self.isWinner = True
    #显示迄今为止所有猜测结果
    def showResults(self, usrNums, results) :
        print '-' * 20
        for i in range(self.count) :
            print '(%d)/t%s/t%s' % (i + 1, usrNums[i], results[i])
        print '-' * 20
        if self.isWinner :
            print 'Total: %d times' % self.count
            print 'Congratulations！ Your are winner !!'
    #运行
    def run(self) :
        self.printRules()
        self.randNumGen()
        while not self.isWinner:
            num = self.numInput()
            self.numJudge(self.sysNum, num)
            self.showResults(self.usrNums, self.results)
#主函数
def main() :
    guessNumber = GuessNumber()
    guessNumber.run()
if __name__ == '__main__' :
    main()
```








