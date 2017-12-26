---
layout: post
title: 'Python学习  文件遍历'
date: 2010-8-5
wordpress_id: 450
permalink: /archives/python-learning-file-traversal.html
categories: [Programming]
tags: [Python]
keywords: "Python"
description: 
comments: true
---
Python的文件遍历主要使用的os这个模块，这里为了方便显示同时使用了一个图形化的库Tkinter。Tkinter是Python自带的一个GUI开发库，虽然没有PyQt或wxPython那么强大，但是基本的使用绝对足够了。

``` python 
#!/usr/bin/env python
#文件遍历GUI
import os
from time import sleep
from Tkinter import *
class DirList(object):
    def __init__(self, initDir = None):
        self.top = Tk()
        self.label = Label(self.top, text = 'Directory Lister')
        self.label.pack()
        
        self.cwd = StringVar(self.top)
        self.dirLabel = Label(self.top, fg = 'blue',
                              font = ('Helvetica', 12, 'bold'))
        self.dirLabel.pack()
        self.dirFrame = Frame(self.top)
        self.dirScrollbar = Scrollbar(self.dirFrame)
        self.dirScrollbar.pack(side = RIGHT, fill = Y)
        self.dirListbox = Listbox(self.dirFrame, height = 15,width = 50,
                        yscrollcommand = self.dirScrollbar.set)
        self.dirListbox.bind('<Double-l>', self.setDirAndShow)
        self.dirScrollbar.config(command = self.dirListbox.yview)
        self.dirListbox.pack(side = LEFT, fill = BOTH)
        self.dirFrame.pack()
        self.dirEntry = Entry(self.top, width = 50,
                              textvariable = self.cwd)
        self.dirEntry.bind('<Return>', self.showList)
        self.dirEntry.pack()
        self.buttonFrame = Frame(self.top)
        self.clearButton = Button(self.buttonFrame, text = 'Clear',
                                  command = self.clearDir,
                                  activeforeground = 'white',
                                  activebackground = 'blue')
        self.listButton = Button(self.buttonFrame, text = 'List Directory',
                                 command = self.showList,
                                 activeforeground = 'white',
                                 activebackground = 'green')
        self.quitButton = Button(self.buttonFrame, text = 'Quit',
                                 command = self.top.quit,
                                 activeforeground = 'white',
                                 activebackground = 'red')
        self.clearButton.pack(side = LEFT)
        self.listButton.pack(side = LEFT)
        self.quitButton.pack(side = LEFT)
        self.buttonFrame.pack()
        if initDir:
            self.cwd.set(os.curdir)
            self.showList()
        
    #清空列表
    def clearDir(self, event = None):
        self.cwd.set('')
    #设置目录并显示
    def setDirAndShow(self, event = None):
        self.lastDir = self.cwd.get()
        self.dirListbox.cofig(selectbackground = 'red')
        check = self.dirListbox.get(self.dirListbox.curselection())
        if not check:
            check = os.curdir
        self.cwd.set(check)
        self.showList()
    #显示列表
    def showList(self, event = None):
        error = ''
        tmp = self.cwd.get()
        if not tmp:
            tmp = os.curdir
        if not os .path.exists(tmp):
            error = tmp + ': no such file'
        elif not os.path.isdir(tmp):
            error = tmp + ':not a directory'
        if error:
            self.cwd.set(error)
            self.top.update()
            sleep(2)
            if not (hasattr(self, 'last') and self.lastDir):
                self.lastDir = os.curdir
                self.cwd.set(self.lastDir)
                self.dirListbox.config(selectbackground = 'LightSkyBlue')
                self.top.update()
                return
        self.cwd.set('Fetching Directory contents...')
        self.top.update()
        dirlist = os.listdir(tmp)
        os.chdir(tmp)
        self.dirLabel.config(text = os.getcwd())
        self.dirListbox.delete(0, END)
        self.dirListbox.insert(END, os.curdir)
        self.dirListbox.insert(END, os.pardir)
        for eachFile in dirlist:
            self.dirListbox.insert(END, eachFile)
            self.cwd.set(os.curdir)
            self.dirListbox.config(selectbackground = 'LightSkyBlue')
#主函数
def main():
    dirList = DirList(os.curdir)
    dirList.top.mainloop()
    dirList.top.destroy()
if __name__ == '__main__':
    main()            
```
