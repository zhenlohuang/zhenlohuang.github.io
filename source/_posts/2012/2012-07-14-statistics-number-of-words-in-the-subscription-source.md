---
layout: post
title: '【集体智慧编程 学习笔记】统计订阅源中的单词数'
date: 2012-7-14
wordpress_id: 3255
permalink: /archives/statistics-number-of-words-in-the-subscription-source.html
categories: [Data Mining]
tags: []
keywords: ""
description: 
comments: true
---
几乎所有的博客都可以在线阅读，或者通过RSS订阅源进行阅读。RSS订阅源是一个包含博客及其所有文章条目信息的简单的XML文档。
程序中使用了feedparser第三方模块，可以轻松地从任何RSS或Atom订阅源中得到标题、链接和文章的条目。完整代码如下：

``` python 
'''
Created on Jul 14, 2012

@Author: killua
@E-mail: killua_hzl@163.com
@Homepage: http://www.yidooo.net
@Decriptioin: Counting the words in a Feed

feedparser:feedparser is a Python library that parses feeds in all known formats, including Atom, RSS, and RDF.It runs on Python 2.4 all the way up to 3.2.

dataset: http://kiwitobes.com/clusters/feedlist.txt
         You can download feeds from this list. Maybe some feeds you can access in China.
'''

import feedparser
import re

#Get word from feed
def getwords(html):
    #Remove all the HTML tags
    text = re.compile(r"<[^>]+>").sub('', html)

    #Split words by all non-alpha characters
    words = re.compile(r"[^A-Z^a-z]+").split(text)

    #Convert words to lowercase
    wordlist = [word.lower() for word in words if word != ""]

    return wordlist

#Returns title and dictionary of word counts for an RSS feed
def getFeedwordcounts(url):
    #Parser the feed
    d = feedparser.parse(url)
    wordcounts = {}

    #Loop over all the entries
    for e in d.entries:
        if 'summary' in e:
            summary = e.summary
        else:
            summary = e.description

    words = getwords(e.title + ' ' + summary)
    for word in words:
        wordcounts.setdefault(word, 0)
        wordcounts[word] += 1

    return d.feed.title, wordcounts

if __name__ == '__main__':
    #count the words appeared in blog
    blogcount = {}
    wordcounts = {}

    feedFile = file('resource/feedlist.txt')
    feedlist = [line for line in feedFile.readlines()]

    for feedUrl in feedlist:
        try:
            title, wc = getFeedwordcounts(feedUrl)
            wordcounts[title] = wc
            for word, count in wc.items():
                blogcount.setdefault(word, 0)
                if count > 1:
                    blogcount[word] += 1
        except:
            print 'Failed to parse feed %s' % feedUrl

    wordlist = []
    for w, bc in blogcount.items():
        frac = float(bc) / len(feedlist)
        if frac > 0.1 and frac < 0.5:
            wordlist.append(w)

    #Write the result to the file
    datafile = file('blogdata', 'w')
    #Write result's head
    datafile.write('Blog')
    for word in wordlist:
        datafile.write('\t%s' % word)
    datafile.write('\n')
    #Write results
    for blogname, wc in wordcounts.items():
        print blogname
        datafile.write(blogname)
        for word in wordlist:
            if word in wc:
                datafile.write("\t%d" % wc[word])
            else:
                datafile.write("\t0")
        datafile.write('\n')
```
