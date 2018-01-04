---
layout: post
title: 'QT学习笔记之十六  ImageSpaceCompute'
date: 2009-7-29
wordpress_id: 332
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
这个软件用于计算文件夹中图片所占空间大小

程序很短，直接看代码吧

**main.cpp**

``` cpp
#include <QtGui>
#include <iostream>
#include <string>
/**
  计算文件中图片所占大小
  */
qlonglong imageSpaceCompute(const QString &path)
{
    QDir dir(path);
    qlonglong size = 0;
    QStringList filters;
    foreach(QByteArray format,QImageReader::supportedImageFormats())
        filters += "*." + format;
    foreach(QString file, dir.entryList(filters,QDir::Files))   //entryList 返回文件夹下满足条件的文件名列表
        size += QFileInfo(dir,file).size();
    //递归，计算子目录中的图片大小
    foreach (QString subDir, dir.entryList(QDir::Dirs | QDir::NoDotAndDotDot))
        size += imageSpaceCompute(path + QDir::separator() + subDir);
    return size;
}
int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);
    QString path = QDir::currentPath();
    std::cout << "Space used by imaged in" << qPrintable(path)
              << " and its subdirectories is " << (imageSpaceCompute(path) / 1024) << " KB" << std::endl;
    return 0;
}
```

要是不能运行看看项目配置文件，这里附上我的

```
QT       = core gui xml
TARGET = ImageSpaceCompute
CONFIG   += console
CONFIG   -= app_bundle
TEMPLATE = app

SOURCES += main.cpp
```
软件截图：

![image](/images/uploads/2009/07/204058014.p.jpg?d=20090729205945389)