---
layout: post
title: 'Zookeeper源码分析-源码编译'
date: 2014-8-31
wordpress_id: 4930
categories: [Distributed System]
tags: [ZooKeeper]
comments: true
---

# 源码下载

``` bash
git clone https://github.com/apache/zookeeper.git
```

# 源码编译
Zookeeper的代码构建采用的是，所以可以直接使用ant命令进行编译。
``` bash
ant
```

# Unit Tests

``` bash
ant -Djavac.args="-Xlint -Xmaxwarns 1000" clean test tar
```

# Troubleshooting
1.在运行unit test的时候出现

```
create-cppunit-configure:
 [exec] configure.ac:37: warning: macro 'AM_PATH_CPPUNIT' not found in library
 [exec] configure.ac:37: error: possibly undefined macro: AM_PATH_CPPUNIT
 [exec] If this token and others are legitimate, please use m4_pattern_allow.
 [exec] See the Autoconf documentation.
 [exec] configure.ac:57: error: possibly undefined macro: AC_PROG_LIBTOOL
 [exec] autoreconf: /usr/bin/autoconf failed with exit status: 1
```

解决方法：

``` bash
sudo apt-get install libtool
```

2.在运行unit test的时候出现

```
create-cppunit-configure:
 [mkdir] Created dir: /home/killua/Workspace/zookeeper/build/test/test-cppunit
 [exec] checking for doxygen... no
 [exec] checking for perl... /usr/bin/perl
 [exec] checking for dot... no
 [exec] configure: error: cannot find install-sh, install.sh, or shtool in "/home/killua/Workspace/zookeeper/src/c" "/home/killua/Workspace/zookeeper/src/c/.." "/home/killua/Workspace/zookeeper/src/c/../.."
```

解决方法：

``` bash
sudo apt-get install doxygen
sudo apt-get install perl
sudo apt-get install shtool
sudo apt-get install graphviz
sudo apt-get install libcppunit-dev
```


