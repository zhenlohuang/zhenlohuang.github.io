---
layout: post
title: 'MDI Editor 部分功能修正'
date: 2009-7-10
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
今天晚上突然想到，前几天写那个MDI Editor 少了拖拽功能支持，这里补充一下....

主要是对Editor类进行补充

在editor.h添加

``` cpp
//拖放
void dragEnterEvent(QDragEnterEvent *event);
void dropEvent(QDropEvent *event);
```
在editor.cpp添加

``` cpp
/**
  拖放的两个Event
  */
void Editor::dragEnterEvent(QDragEnterEvent *event)
{
    if(event->mimeData()->hasFormat("text/uri-list"))
        event->acceptProposedAction();
}
void Editor::dropEvent(QDropEvent *event)
{
    QList<QUrl> urls = event->mimeData()->urls();
    if(urls.isEmpty())
        return ;
    QString fileName = urls.first().toLocalFile();
    if (fileName.isEmpty())
        return ;
    if(readFile(fileName))
        setWindowTitle(tr("%1").arg(fileName));
}
```
OK.

PS：晚上才发现，我的工程名居然打错了..mdieditor 打成mideditor了...好尴尬...算了不改了，有不影响学习交流
