---
layout: post
title: 'Python学习  多线程编程  生产者-消费者问题'
date: 2010-8-5
wordpress_id: 458
permalink: /archives/python-learning-multithreaded-programming-producer-consumer-problem.html
categories: [Programming]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
生产者-消费者问题是系统进程里面的一个经典问题，这里用Python简单模拟一下。

``` python 
#!/usr/bin/env python
#生产者-消费者问题
import threading
from random import randint
from time import sleep, ctime
from Queue import Queue
#创建MyThread子类
class MyThread(threading.Thread):
    def __init__(self, func, args, name = ''):
        threading.Thread.__init__(self)
        self.name = name
        self.func = func
        self.args = args
    def getResult(self):
        return self.res
    def run(self):
        print'starting', self.name, 'at:', ctime()
        self.res = apply(self.func, self.args)
        print self.name, 'finished at:', ctime()
#生产者与消费者问题
def writeQueue(queue):
    queue.put('Anything', 1)
    print "Producing object for Queue. Size now", queue.qsize()
def readQueue(queue):
    val = queue.get(1)
    print 'Consumed object from Queue. Size now', queue.qsize()
def produce(queue, loops):
    for i in range(loops):
        writeQueue(queue)
        sleep(randint(1,3))
def consume(queue, loops):
    for i in range(loops):
        readQueue(queue)
        sleep(randint(2,5))
def main():
    funcs = [produce, consume]
    nfuncs = range(len(funcs))
    nloops = randint(2, 5)
    queue = Queue(32)
    threads = []
    for i in nfuncs:
        t = MyThread(funcs[i], (queue, nloops),funcs[i].__name__)
        threads.append(t)
    for i in nfuncs:
        threads[i].start()
    for i in nfuncs:
        threads[i].join()
    print 'All Done!!'
if __name__ == '__main__':
    main() 
```
