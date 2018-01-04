---
layout: post
title: 'Python学习  网络编程（五）  FTP连接'
date: 2010-8-5
wordpress_id: 459
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
Python里面的FTP连接，主要依赖ftplib这个模块，具体请看帮助文档。

``` python 
#!/usr/bin/env python
#FTP下载程序
import ftplib
import os
import socket
HOST = 'ftp.mozilla.org'
DIR = 'pub/mozilla.org/webtools'
FILE = 'bugzilla-LATEST.tar.gz'
def ftpDownload() :
    try:
        f = ftplib.FTP(HOST)
    except (socket.error, socket.gaierror), e:
        print 'ERROR: cannot connect "%s"' % HOST
        return
    print '>>Connect to host "%s"' % HOST
    try:
        f.login()
    except ftplib.error_perm:
        print 'ERROR: cannot login anonymously'
        f.quit()
        return
    print '>>Logged in as "anonymous"'
    try:
        f.cwd(DIR)
    except ftplib.error_perm:
        print 'ERROR: cannot go to "%s"' % DIR
        f.quit()
        return
    print '>>Go to "%s"' % DIR
    try:
        f.retrbinary('RETR %s' % FILE,
                     open(FILE, 'wb').write)
    except ftplib.error_perm:
        print 'ERROR: cannot read file "%s"' % FILE
        os.unlink(FILE)
    else:
        print '>>Download "%s"' % FILE
        f.quit()
        return
def main() :
    ftpDownload()
if __name__ == '__main__':
    main() 
```
