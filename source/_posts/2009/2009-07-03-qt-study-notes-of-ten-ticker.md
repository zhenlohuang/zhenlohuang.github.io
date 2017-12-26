---
layout: post
title: 'QT学习笔记之十  Ticker'
date: 2009-7-3
wordpress_id: 313
permalink: /archives/qt-study-notes-of-ten-ticker.html
categories: [Programming]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---

无聊学习着......

写一个Ticker，练习下事件触发

部分代码如下：

**ticker.h**

``` cpp
#ifndef TICKER_H
#define TICKER_H
#include <QtGui/QWidget>
namespace Ui
{
    class Ticker;
}
class Ticker : public QWidget
{
    Q_OBJECT
public:
    Ticker(QWidget *parent = 0);
    ~Ticker();
    void setText(const QString &newText);
    QString text() const ;
    QSize sizeHint() const;
protected:
    void paintEvent(QPaintEvent *event);
    void timerEvent(QTimerEvent *evnet);
    void showEvent(QShowEvent *event);
    void hideEvent(QHideEvent *event);
private:
    Ui::Ticker *ui;
    QString tickerText;
    int offset;    //时间间隔
    int timerID;   //定时器ID
};
#endif // TICKER_H
```

**ticker.cpp**

``` cpp
*#include <QtGui>
#include "ticker.h"
#include "ui_ticker.h"
Ticker::Ticker(QWidget *parent)
    : QWidget(parent), ui(new Ui::Ticker)
{
    ui->setupUi(this);
    offset = 0;
    timerID = 0;
}
Ticker::~Ticker()
{
    delete ui;
}
/**
  设置文本
  */
void Ticker::setText(const QString &newText)
{
    tickerText = newText;
    update();
    updateGeometry();
}
/**
  获取文本内容
  */
QString Ticker::text() const
{
    return tickerText;
}
/**
  设置Widget大小
  */
QSize Ticker::sizeHint() const
{
    return fontMetrics().size(0,text());   //fontMetrics(): Returns the font metrics for the widget's current font
}
/**
  绘制事件
  */
void Ticker::paintEvent(QPaintEvent *)
{
    QPainter painter(this);
    int textWidth = fontMetrics().width(text());
    if (textWidth < 1)
        return ;
    int x = -offset;
    while (x < width()) {
        painter.drawText(x,0,textWidth,height(),Qt::AlignLeft | Qt::AlignVCenter,text());   //绘制文本
        x += textWidth;
    }
}
/**
  定时器事件
  */
void Ticker::timerEvent(QTimerEvent *event)
{
    if (event->timerId() == timerID) {
        ++offset;
        if(offset >= fontMetrics().width(text()))
            offset = 0;
        scroll(-1,0);    //向左滚动一个像素
    } else {
        QWidget::timerEvent(event);
    }
}
/**
  显示事件
  */
void Ticker::showEvent(QShowEvent *)
{
    timerID = startTimer(30);   //Starts a timer and returns a timer identifier, or returns zero if it could not start a timer.
}
/**
  隐藏事件
  */
void Ticker::hideEvent(QHideEvent *)
{
    killTimer(timerID);
    timerID = 0;
}
```

**界面截图如下：**

![image](/images/uploads/2009/07/171211091.p.png?d=20090705171255123)


可以自动向左滚动...（怎么感觉像楼下显示电费的....尴尬了）

---

继续无聊的暑假....
