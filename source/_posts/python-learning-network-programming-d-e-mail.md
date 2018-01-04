---
layout: post
title: 'Python学习 网络编程（四） E-Mail'
date: 2010-8-5
wordpress_id: 456
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
E-Mail的收发涉及到STMP和POP3两个协议。下面的代码演示了STMP和POP3连接的建立

``` python 
#!/usr/bin/env python
#E-Mail客户端
from smtplib import SMTP
from poplib import POP3
from time import sleep
SMTPSVR = 'stmp.163.com'
POP3SVR = 'pop.163.com'
FROMMAIL = 'killua_hzl@163.com'
TOMAIL = 'killua_hzl@163.com'
origHdrs = ['From: %s' % FROMMAIL,
            'To: %s' % TOMAIL,
            'Subject: Just for test']
origBody = ['Test1','Test2','Test3']
origMsg = '/r/n/r/n'.join(['/r/n'.join(origHdrs),
                           '/r/n'.join(origBody)])
sendSvr = SMTP(SMTPSVR)
errs = sendSvr.sendmail(FROMMAIL, TOMAIL, origMsg)
sendSvr.quit()
assert len(errs) == 0, errs
sleep(10)   #wait for mail to be delivered
recvSvr  = POP3(POP3SVR)
recvSvr.user('killua_hzl')
recvSvr.pass_('123456')
rsp, msg, size = recvSvr.retr(recvSvr.stat()[0])
sep = msg.index('')
recvBody = msg[sep + 1]
assert origBody == recvBody 
```
