---
layout: post
title: 'PyLucene学习笔记 文件索引及检索'
date: 2011-11-21
wordpress_id: 523
permalink: /archives/pylucene-study-notes-document-indexing-and-retrieval.html
categories: [Search Engine]
tags: []
keywords: ""
description: 
comments: true
---
<div style="word-break: break-word; word-wrap: break-word; overflow-y: auto; padding: 0px; margin: 5px;">

一、使用Indexer建立文本文件索引

这里简化为对某一目录下面的所有后缀为“.py”的文件建立索引。

``` python 
'''
Created on 2011-11-16

@author: killua
@E-mail:killua_hzl@163.com
'''
import os

from time import time
from datetime import timedelta
from lucene import \
    IndexWriter, StandardAnalyzer, Document, Field, \
    InputStreamReader, FileInputStream, Version, SimpleFSDirectory, File, \
    initVM, CLASSPATH
    
class Indexer(object):
    _indexDir = ""
    _dataDir = ""
        
    def __init__(self, indexDir, dataDir):
        self._indexDir = indexDir
        self._dataDir = dataDir
        
    def index(self):
        if not (os.path.exists(self._dataDir) and os.path.isdir(self._dataDir)):
            raise IOError, "%s isn't existed or is not a directory" % (self._dataDir)
        
        dir = SimpleFSDirectory(File(self._indexDir))
        writer = IndexWriter(dir, StandardAnalyzer(Version.LUCENE_CURRENT),
                             True, IndexWriter.MaxFieldLength.LIMITED)
        writer.setUseCompoundFile(False);
        self.indexDirectory(writer, self._dataDir)
        numIndexed = writer.numDocs();
        writer.optimize()
        writer.close()
        dir.close()
        
        return numIndexed
    
    def indexDirectory(self, writer, dir):
        
        for name in os.listdir(dir):
            path = os.path.join(dir, name)
            if os.path.isfile(path):
                if path.endswith('.py'):
                    self.indexFile(writer, path)
            elif os.path.isdir(path):
                self.indexDirectory(writer, path)
             
    def indexFile(self, writer, path):
        try:
            reader = InputStreamReader(FileInputStream(path), 'UTF-8')
        except IOError, e:
            print 'IOError while opening %s: %s' %(path, e)
        else:
            print 'Indexing', path
            doc = Document()
            doc.add(Field("contents", reader))
            doc.add(Field("path", os.path.abspath(path),
                          Field.Store.YES, Field.Index.NOT_ANALYZED))
            writer.addDocument(doc)
            reader.close()

def main():
    initVM(CLASSPATH)
    indexDir = "./index/"
    dataDir = "/home/killua/Workspace/"
    indexer = Indexer(indexDir, dataDir)
    
    start = time()
    numIndexed = indexer.index()
    duration = timedelta(seconds=time() - start)
    print "Indexing %s files took %s" %(numIndexed, duration)
    
if __name__ == "__main__":
    main()
```

运行结果：

Indexer.py 将在index目录下建立一些索引文件，用于对.py文件建立索引

二、使用Searcher检索文件

下面程序中实现了对文件的检索，其中参数q是一个表达式，可以理解为检索使用的关键字，如果文件中包含次关键字将被检索出来

``` python
'''
Created on 2011-11-17

@author: killua
@E-mail:killua_hzl@163.com
'''

import os

from time import time
from datetime import timedelta

from lucene import \
     Document, IndexSearcher, FSDirectory, QueryParser, StandardAnalyzer, \
     SimpleFSDirectory, File, Version, initVM, CLASSPATH
     
class Searcher(object):
    
    def search(self, indexDir, q):
        fsDir = SimpleFSDirectory(File(indexDir))
        searcher = IndexSearcher(fsDir, True)
        query = QueryParser(Version.LUCENE_CURRENT, "contents",
                            StandardAnalyzer(Version.LUCENE_CURRENT)).parse(q)
        starttime = time()
        hits = searcher.search(query, 50).scoreDocs
        duration = timedelta(seconds=time() - starttime)
        
        print "Found %d document(s) (in%s) that matched query '%s':" %(len(hits), duration, q)
        
        for hit in hits:
            doc = searcher.doc(hit.doc)
            print 'path:', doc.get("path")
 
def main():
    initVM(CLASSPATH)
    indexDir = "./index/"
    q = "Killua"
    searcher = Searcher()
    searcher.search(indexDir, q)
    
if __name__ == "__main__":
    main()
```

