---
layout: post
title: 'Python学习笔记 OS模块'
date: 2011-8-3
categories: [Programming, Python]
tags: [Python]
keywords: "Python"
description: 
comments: true
---

``` python 
'''
Created on 2011-8-1

@author: Killua
@E-mail:killua_hzl@163.com
@Description:
'''

#!/usr/bin/env python3

import os
import time

#使用os模块进行文本替换
def context_replace(file, search_for, replace_with, new_file = 'new_file'):
    try:
        #remove old temp
        os.remove(new_file)
    except os.error:
        pass

    #open files
    fi = open(file)
    fo = open(new_file, 'w')

    #replace context
    for line in fi.readlines():
        fo.write(line.replace(search_for, replace_with))

    #close files
    fi.close()
    fo.close()

#使用 os 列出目录下的文件
def file_list(filepath):
    for filename in os.listdir(filepath):
        print(filename)

#使用 os 模块查看当前工作目录
def current_word_dir():
    print("Currnet Directory:" + os.getcwd())    

#使用 os 模块创建/删除目录
def make_dir(dir_name):
    os.mkdir(dir_name)

def delete_dir(dir_name):
    if not os.path.isdir(dir_name):
        print("It's not a directory")
        return 
    else:
        if len(os.listdir(dir_name)) == 0:
            os.rmdir(dir_name)
        else:
            print("The directory you want to delete is not empty,")

#使用 os 模块获取文件属性
def get_file_info(filename):
    file_state = os.stat(filename)
    mode, ino, dev, nlink, uid, gid, size, atime, mtime, ctime = file_state
    print("size:", size, "bytes")
    print("owner:", uid, gid)
    print("created:", time.ctime(ctime))
    print("last accessed:", time.ctime(atime))
    print("last modified:", time.ctime(mtime))
    print("mode:", oct(mode))
    print("inode/dev", ino, dev)

#使用 os 执行操作系统命令
def os_command_excute(cmd):
    os_name = os.name 
    if os_name == "nt":
        print("Windows Command")
    else:
        print("Unix/Linux Command")
    os.system(cmd)    

#Test
def main():
    #===Just For Test===
    #context_replace("sample", 'a', 'A')
    #file_list('/')
    #current_word_dir()
    #make_dir('sample_dir')
    #delete_dir('sample_dir')
    #get_file_info("sample") 
    os_command_excute("ls -l")  

if __name__ == "__main__":
    main()
```
