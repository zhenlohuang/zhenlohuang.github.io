---
layout: post
title: 'Python学习笔记 OS.Path模块'
date: 2011-8-3
wordpress_id: 512
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---

``` python 
'''
Created on 2011-8-3

@author: Killua
@E-mail:killua_hzl@163.com
@Description:
'''
#!/usr/bin/env python3

import os

#使用 os.path 模块处理文件名
def file_path_info(filename):
    print("OS:" + os.name)
    print("split:", os.path.split(filename))
    print("splitext:", os.path.splitext(filename))
    print("dirname:", os.path.basename(filename))
    print("basename:" + os.path.basename(filename))

#使用 os.path 搜索文件系统
def list_files(directory):
    stack = [directory]
    files = []
    while stack:
        directory = stack.pop()
        for file in os.listdir(directory):
            fullname = os.path.join(directory, file)
            files.append(fullname)
            if os.path.isdir(fullname) and not os.path.islink(fullname):
                stack.append(fullname)
    #List Files
    for file in files:
        print(file)

def main():
    #Just For Test
    #file_path_info("~/Workspace/MyProject/src/os_path_demo/os_path_demo.py")
    list_files("/home/killua")

if __name__ == "__main__":
    main()
```
