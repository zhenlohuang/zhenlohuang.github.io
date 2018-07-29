---
layout: post
title: 'PyLucene安装'
date: 2011-5-16
categories: [Search Engine]
tags: [PyLucene]
keywords: "PyLucene"
description: 
comments: true
---

1.安装Python

``` 
 sudo apt-get install python
 sudo apt-get install python-dev
```

2.安装Ant

```
 sudo apt-get install ant
```

3.安装setuptools

```
 sudo apt-get install python-setuptools
```

 打补丁：

```
 mkdir tmp
 cd tmp
 unzip -q /usr/local/lib/python2.6/dist-packages/setuptools-0.6c11-py2.6.egg
 patch -Nup0 < /home/killua/Download/pylucene-3.1.0-1/jcc/jcc/patches/patch.43.0.6c11
 sudo zip /usr/local/lib/python2.6/dist-packages/setuptools-0.6c11-py2.6.egg -f
 cd com, and it provides a fantastic selection of  <a href="http://slotmachineitaliane.net">slotmachineitaliane.net</a>  games, bonuses, promotions and more. ..
 rm -rf tmp
```

4.安装JDK

```
 sudo apt-get install openjdk-6-jdk
```

5.安装Pylucene
 下载地址：<http://www.apache.org/dyn/closer.cgi/lucene/pylucene/>
 tar xzvf pylucene-3.0.1-1-src.tar.gz
 cd pylucene-3.0.1-1/jcc

6.安装jcc

```
 python setup.py build
 sudo python setup.py install
```

7.修改Makeifle
 打开了这几行注释：

``` bash
 # Linux   (Ubuntu 8.10 64-bit, Python 2.5.2, OpenJDK 1.6, setuptools 0.6c9)
  PREFIX_PYTHON=/usr
  ANT=ant
  PYTHON=$(PREFIX_PYTHON)/bin/python
  JCC=$(PYTHON) -m jcc --shared
  NUM_FILES=3
```
 修改-m jcc 为 -m jcc.__main__

8.编译安装Pylucene

```
 make
 sudo make install
```