---
layout: post
title: 'Python学习  网络编程（二） UDP连接'
date: 2010-8-5
wordpress_id: 454
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
这里定义了一个UDPServer和UDPClient。这里创建一个TCP服务程序，服务器会把客户发送过来的字符串加上一个时间戳，然后显示,并返回客户端。

**UDPServer.py**

``` python 
#!/usr/bin/env python
from socket import *
from time import ctime
#创建一个UDP客户端
HOST = ''
PORT = 20001
BUFSIZE = 1024
ADDR = (HOST, PORT)
udpSerSock = socket(AF_INET, SOCK_DGRAM)
udpSerSockbind(ADDR)
while True:
    print 'waiting for message...'
    data, addr = udpSerSock.recvfrom(BUFSIZE)
    udpSerSock.sendto('[%s] %s' % (ctime(), data), addr)
    print'received from %s >> %s' % (addr, data)
udpSerSock.close()
```

**UDPClient.py**

``` python
#!/usr/bin/env python
from socket import *
HOST = 'localhost'
PORT = 20001
BUFSIZE = 1024
ADDR = (HOST, PORT)
udpClientSock = socket(AF_INET, SOCK_DGRAM)
while True:
    data = raw_input('Enter the message you want to send >')
    if not data:
        break
    udpClientSock.sendto(data, ADDR)
    data, ADDR = udpClientSock.recvfrom(BUFSIZE)
    if not data:
        break
    print data
udpClientSock.close()
```


