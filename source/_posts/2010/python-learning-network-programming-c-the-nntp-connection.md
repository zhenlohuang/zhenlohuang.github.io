---
layout: post
title: 'Python学习 网络编程（三） NNTP连接'
date: 2010-8-5
wordpress_id: 455
categories: [Programming]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
NNTP连接主要是用在新闻组的消息上面，下面的代码演示了NNTP连接的建立，不过显然下面的那个新闻组客户端程序是不能用的。

``` python 
#!/usr/bin/env python
#新闻组客户端程序
import nntplib
import socket
HOST = 'your.nntp.server'
GRNM = 'comp.lang.python'
USERNAME = 'Killua'
PASSWORD = '123456'
def NNTPClient():
    try:
        n = nntplib.NNTP(HOST)
    except socket.gaierror, e:
        print 'ERROR: cannot connect host "%s"' % HOST
        print '("%s")' % eval(str(e))[1]
        return
    except nntplib.NNTPPermanentError, e:
        print 'ERROR: access denied on "%s"' % HOST
        print '("%s")' % str(e)
        return
    print '>> Connected to host "%s"' % HOST
    try:
        rsp, ct, fst, lst, grp = n.group(GRNM)
    except nntplib.NNTPTemporaryError, e:
        print 'ERROR: cannot load group "%s"' % GRNM
        print '("%s")' % str(e)
        print 'Server may require authentication'
        print 'Uncomment/Edit login lne above'
        n.quit()
        return
    except nntplib.NNTPTemporaryError, e:
        print 'ERROR: group "%s" unavailable' % GRNM
        print '("%s")' % str(e)
        n.quit()
        return
    print '>> Found newgroup "%s"' % GRNM
    rng = '%s-%s' % (lst,lst)
    rsp, frm = n.xhdr('from', rng)
    rsp, sub = n.xhdr('subjec', rng)
    rsp, dat = n.xhdr('date', rng)
    print ''' Found last article(#%s):
From: %s
Subject: %s
Date: %s
''' (lst, frm[0][1], sub[0][1], dat[0][1])
def displayFirst20(data):
    'Display First 20 lines'
    count = 0
    lines = (line.rstrip() for line in data)
    lastBlank = True
    for line in lines:
        if line:
            lower = line.lower()
            if (lower.startswith('>') and /
                not lower.startswith('>>>')) or /
                lower.startswith('|') or /
                lower.startswith('in article') or /
                lower.endswith('writes:') or /
                lower.endswith('wrote:'):
                continue
        if not lastBlank or (lastBlank and line):
            print '%s' % line
        if line:
            count += 1
            lastBlank = False
        else:
            lastBlank = True
        if count == 20:
            break
def main():
    NNTPClient()
    DisplayFirst20()
if __name__ == '__main__':
    main() 
```
