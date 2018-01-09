---
layout: post
title: 'QT学习笔记之十八  Threads Qt多线程练习'
date: 2009-8-5
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
多线程这个部分看得晕晕的....

随便搞了段代码...

===================================================================

部分代码如下：

**threads.h**

``` cpp 
#ifndef THREADS_H
#define THREADS_H
#include <QThread>
class Threads : public QThread
{
    Q_OBJECT
public:
    Threads();
    void setMessage(const QString &message);
    void stop();
protected:
    void run();
private:
    QString messageStr;
    volatile bool stopped;
};
#endif // THREADS_H 
```

**threads.cpp**

``` cpp 
#include <QtCore>
#include <iostream>
#include "threads.h"
Threads::Threads()
{
    stopped = false;
}
void Threads::setMessage(const QString &message)
{
    messageStr = message;
}
void Threads::run()
{
    while(!stopped)
        std::cout << qPrintable(messageStr) <<std::endl;
}
void Threads::stop()
{
    stopped = true;
} 
```

**threadDialog.h**

``` cpp 
#ifndef THREADDIALOG_H
#define THREADDIALOG_H
#include <QDialog>
#include "threads.h"
class ThreadDialog : public QDialog
{
    Q_OBJECT
public:
    ThreadDialog(QWidget *parent = 0);
protected:
    void closeEvent(QCloseEvent *event);
private slots:
    void startOrStopThreadA();
    void startOrStopThreadB();
private:
    Threads threadA;
    Threads threadB;
    QPushButton *threadAButton;
    QPushButton *threadBButton;
    QPushButton *quitButton;
};
#endif // THREADDIALOG_H
```

**threadDialog.cpp**

``` cpp 
#include <QtGui>
#include "threaddialog.h"
ThreadDialog::ThreadDialog(QWidget *parent) : QDialog(parent)
{
    threadA.setMessage("Thread A is start");
    threadB.setMessage("Thread B is start");
    threadAButton = new QPushButton(tr("Start A"));
    threadBButton = new QPushButton(tr("Start B"));
    quitButton = new QPushButton(tr("Quit"));
    quitButton->setDefault(true);
    connect(threadAButton, SIGNAL(clicked()),this, SLOT(startOrStopThreadA()));
    connect(threadBButton,SIGNAL(clicked()), this, SLOT(startOrStopThreadB()));
    connect(quitButton, SIGNAL(clicked()), this, SLOT(close()));
    QHBoxLayout *layout = new QHBoxLayout;
    layout->addWidget(threadAButton);
    layout->addWidget(threadBButton);
    layout->addWidget(quitButton);
    setLayout(layout);
    setWindowTitle(tr("Thread Test"));
}
void ThreadDialog::startOrStopThreadA()
{
    if (threadA.isRunning()) {
        threadA.stop();
        threadAButton->setText(tr("Start A"));
    } else {
        threadA.start();
        threadAButton->setText(tr("Stop A"));
    }
}
void ThreadDialog::startOrStopThreadB()
{
    if (threadB.isRunning()) {
        threadB.stop();
        threadBButton->setText(tr("Start B"));
    } else {
        threadB.start();
        threadBButton->setText(tr("Stop B"));
    }
}
void ThreadDialog::closeEvent(QCloseEvent *event)
{
    threadA.stop();
    threadB.stop();
    threadA.wait();
    threadB.wait();
    event->accept();
}
```

这个程序没有优化，感觉有点站内存的说，看看就好哈
