---
layout: post
title: 'Python学习 网络编程（一） TCP连接'
date: 2010-8-5
wordpress_id: 453
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
这里定义了一个TCPServer和TCPClient。这里创建一个TCP服务程序，服务器会把客户发送过来的字符串加上一个时间戳，然后显示,并返回客户端。主要后面无论如何都要记得close()关上连接，虽然基本上不会执行那一句。

**TCPServer.py**

``` python 
#!/usr/bin/env python
#创建一个TCP服务程序，这个程序会把客户发送过来的字符串加上一个时间戳，然后显示,并返回客户端
from socket import *
from time import ctime
HOST = ''
PORT = 20000     
BUFSIZE = 1024    #1KB
ADDR = (HOST, PORT)
tcpSerSock = socket(AF_INET, SOCK_STREAM)
tcpSerSock.bind(ADDR)
tcpSerSock.listen(5)
while True:
    print 'waiting for connection...'
    tcpClientSock,clientAddr = tcpSerSock.accept()
    print '...connected from :', clientAddr
    while True:
        data = tcpClientSock.recv(BUFSIZE)
        if not data:
            break
        print '[%s] %s' % (ctime(), data)
        tcpClientSock.send('[%s] %s' % (ctime(), data))
    tcpClientSock.close()
tcpSerSock.close()
```

**TCPClient.py**

``` python
#!/usr/bin/env python
#创建一个TCP客户端
from socket import *
HOST = 'localhost'
PORT = 20000
BUFSIZE = 1024
ADDR = (HOST, PORT)
tcpClientSock = socket(AF_INET, SOCK_STREAM)
tcpClientSock.connect(ADDR)
while True:
    data = raw_input('Enter a string your want to send >')
    if not data:
        break
    tcpClientSock.send(data)
    data = tcpClientSock.recv(BUFSIZE)
    if not data:
        break
    print data
tcpClientSock.close() 
```
