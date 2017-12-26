---
layout: post
title: 'Python学习  网络编程（六） 网络爬虫'
date: 2010-8-5
wordpress_id: 460
permalink: /archives/python-learning-network-programming-6-web-crawler.html
categories: [Programming]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
所谓的网络爬虫就是利用程序抓取想要的网页或者数据。

下面对程序中所使用模块进行简单分析：

网络方面涉及Python的三个模块htmllib，urllib，urlparse。   
1）*htmllib*这个模块定义了一个可以担当在超文本标记语言(HTML)中解析文本格式文件的基类。该类不直接与I/O有关--它必须被提供字符串格式的输入，并且调用一个"格式设置"对象的方法来产生输出。该HTMLParser类被设计用来作为其他类增加功能性的基类，并且允许它的多数方法被扩展或者重载。该HTMLParser实现支持HTML 2.0（描述在RFC1866中）语言。   
2）*urllib*模块提供的上层接口，使我们可以像读取本地文件一样读取www和ftp上的数据。通过简单的函数调用，URL所定位的资源就可以被你作为输入使用到你的程序中。如果再配以re(正则表达式)模块，那么你就能够下载Web页面、提取信息、自动创建你所寻找的东西。urlretrieve方法直接将远程数据下载到本地。参数filename指定了保存到本地的路径（如果未指定该参数，urllib会生成一个临时文件来保存数据）；   
3）*urlparse*模块使我们能够轻松地把URL分解成元件（urlparse），之后，还能将这些元件重新组装成一个URL（urljoin）。当我们处理HTML 文档的时候，这项功能是非常方便的。   

系统方面涉及Python的模块比较简单，主要是常用的*sys*和*os*两个模块。主要用于文件夹的创建，文件路径的定位和判断等常用功能。这里不再介绍深入介绍。

输入输出方面使用了*cStringIO*模块。StringIO的行为与file对象非常像，但它不是针对磁盘上文件，而是一个内存中的"文件"，我们可以将操作磁盘文件那样来操作StringIO。

*formatter*模块主要用于格式化输出。这里的格式化输出不仅仅是"格式化"输出而已。它可以将 HTML 解析器的标签和数据流转换为适合输出设备的事件流( event stream ),从 而 写将事件流相应的输出到设备上。

这个程序所使用的Python解释器版本为2.6.5

下面是代码部分
**NetCrawl.py**

``` python 
#!/usr/bin/env python
#网络爬虫程序
from sys import argv
from os import makedirs, unlink, sep
from os.path import dirname, exists, isdir, splitext
from string import replace, find, lower
from htmllib import HTMLParser
from urllib import urlretrieve
from urlparse import urlparse, urljoin
from formatter import DumbWriter, AbstractFormatter
from cStringIO import StringIO
class Downloader(object) :
    #构造器
    def __init__(self, url) :
        self.url = url
        self.file = self.filename(url)
    #分析获取文件名
    def filename(self, url, defFile = 'index.htm') :
        parsedUrl = urlparse(url, 'http:', 0)
        path = parsedUrl[1] + parsedUrl[2]
        ext = splitext(path)
        if ext[1] == '':        #使用默认文件名
            if path[-1] == '/':
                path += defFile
            else:
                path += '/' + defFile
        localDir = dirname(path)
        if sep != '/':
            localDir = replace(localDir, '/', sep)
        if not isdir(localDir):
            if exists(localDir):     #文件存在删除
                unlink(localDir)
            makedirs(localDir)
        return path
    #下载页面
    def download(self) :
        try :
            retval = urlretrieve(self.url, self.file)
        except IOError:
            retval = ('***ERROR: invalid URL "%s"' % self.url)
        return retval
    #分析保存链接
    def parseAndGetLinks(self) :
        self.parser = HTMLParser(AbstractFormatter( /
            DumbWriter(StringIO())))
        self.parser.feed(open(self.file).read())
        self.parser.close()
        return self.parser.anchorlist
class NetCrawler(object):
    count = 0  #计数器
    #构造器
    def __init__(self, url) :
        self.queue = [url]
        self.seen = []
        self.dom = urlparse(url)[1]
    #获取页面
    def getPage(self, url) :
        dl = Downloader(url)
        retval = dl.download()
        if retval[0] == '*' :
            print retval, '...skipping parse'
            return
        NetCrawler.count += 1
        print '/n(', NetCrawler.count, ')'
        print 'Url: ', url
        print 'File: ', retval[0]
        self.seen.append(url)
        links = dl.parseAndGetLinks()
        for eachLink in links :
            if eachLink[ : 4] != 'http' and /
               find(eachLink, '://') == -1:
                eachLink = urljoin(url, eachLink)
                print '*',eachLink
            if find(lower(eachLink), 'mailto:') != -1:
                print '... discarded, mailto link'
                continue
            if eachLink not in self.seen:
                if find(eachLink, self.dom) == -1 :
                    print '... discarded, not in domain'
                else :
                    if eachLink not in self.queue :
                        self.queue.append(eachLink)
                        print '... new, added to queue'
                    else :
                        print '... dirscarded, already in queue'
            else :
                print '... discarded, already processed'
    #处理队列中的链接
    def run(self) :
        while self.queue :
            url = self.queue.pop()
            self.getPage(url)
#主函数
def main() :
    #url = 'http://www.hao123.com/haoserver/index.htm'
    if len(argv) > 1 :
        url = argv[1]
    else :
        try :
            url = raw_input('Enter starting URL: ')
        expect (KeyboardInterrupt, EOFError) :
            url = ''
    if not url :
        return
    netCrawler = NetCrawler(url)
    netCrawler.run()
        
if __name__ == '__main__' :
    main()
```


这里的程序只是简单的抓取网页，如果要抓取指定网页可以加上也正则表达式（模块）来进行处理



