---
layout: post
title: 'QT学习笔记之十一  Timer'
date: 2009-7-5
wordpress_id: 314
categories: [Programming, C++]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---
系统：Ubuntu 9.04

Qt版本：4.5.1

开发工具：Qt Creator 1.1

该软件，主要实现一个定时器的功能，最多可以定时一个小时，界面有点丑，大家不要鄙视阿....


部分代码如下：

**timer.h**

```
#ifndef TIMER_H
#define TIMER_H
#include <QtGui/QWidget>
#include <QDateTime>
class QTimer;
namespace Ui
{
    class Timer;
}
class Timer : public QWidget
{
    Q_OBJECT
public:
    Timer(QWidget *parent = 0);
    ~Timer();
    void setTimer(int secs);
    int getTimer() const;
    void draw(QPainter *painter);
signals:
    void timeout();
protected:
    void paintEvent(QPaintEvent *event);
    void mousePressEvent(QMouseEvent *event);
private:
    Ui::Timer *ui;
    QDateTime finishTime;
    QTimer *updateTimer;
    QTimer *finishTimer;
};
#endif // TIMER_H
```
**timer.cpp**

``` cpp
#include <QtGui>
#include <cmath>
#include "timer.h"
#include "ui_timer.h"
#ifndef PI
#define PI acos(-1)
#endif
const double DegressPerMinute = 6.0;   //每分钟转过6度
const double DegressPerSecond = DegressPerMinute / 60;  //每秒转过7/60度
const int MaxMinutes = 60;   //最大分钟数
const int MaxSeconds = MaxMinutes * 60;  //最大秒钟数
const int UpdateInterval = 5;   //定时器显示更新间隔
/**
  构造函数
  */
Timer::Timer(QWidget *parent)
    : QWidget(parent), ui(new Ui::Timer)
{
    ui->setupUi(this);
    finishTime = QDateTime::currentDateTime();
    updateTimer = new QTimer(this);
    connect(updateTimer,SIGNAL(timeout()),this,SLOT(update()));   //updateTimer 到时间自动更新
    finishTimer = new QTimer(this);
    finishTimer->setSingleShot(true);    //This property holds whether the timer is a single-shot timer.
    connect(finishTimer,SIGNAL(timeout()),this,SIGNAL(timeout()));
    connect(finishTimer,SIGNAL(timeout()),updateTimer,SLOT(stop()));  //到时间，updateTimer 停止
    //字体设置
    QFont font;
    font.setPointSize(8);
    setFont(font);
}
/**
  析构函数
  */
Timer::~Timer()
{
    delete ui;
}
/**
  设置定时器时间
  */
void Timer::setTimer(int secs)
{
    secs = qBound(0, secs, MaxSeconds);  //Returns value bounded by min and max. This is equivalent to qMax(min, qMin(value, max)).
    finishTime = QDateTime::currentDateTime().addSecs(secs);
    if(secs > 0) {
        updateTimer->start(UpdateInterval * 1000);  //单位是ms，所以要乘以1000
        finishTimer->start(secs * 1000);
    } else {
        updateTimer->stop();
        finishTimer->stop();
    }
    update();
}
/**
  返回剩余时间
  */
int Timer::getTimer() const
{
    int secs = QDateTime::currentDateTime().secsTo(finishTime);  //返回两个日期相差的秒数
    if(secs < 0)
        secs = 0;
    return secs;
}
/**
  鼠标按下事件
  */
void Timer::mousePressEvent(QMouseEvent *event)
{
    QPointF point = event->pos() - rect().center();
    double angle = std::atan2(-point.x(), -point.y()) * 180.0 /PI;
    setTimer(getTimer() + int(angle / DegressPerSecond));
    update();
}
/**
  绘制事件
  */
void Timer::paintEvent(QPaintEvent *)
{
    QPainter painter(this);
    painter.setRenderHint(QPainter::Antialiasing,true);  //Sets the given render hint on the painter if on is true; otherwise clears the render hint.
    int side = qMin(width(),height());    //边长
    painter.setViewport((width() - side) / 2,(height() - side) / 2, side, side); //设置显示格式，使其为一个正方形
    painter.setWindow(-50,-50,100,100);  //设置窗口坐标系
    draw(&painter);
}
/**
  绘制定时器
  */
void Timer::draw(QPainter *painter)
{
    static const int triangle[3][2] = { {-2,-49} ,{2,-49}, {0,-47} };
    QPen thickPen(palette().foreground(),1.5);   //palette：调色板
    QPen thinPen(palette().foreground(),0.5);
    //绘制定时器顶部的三角形
    painter->setPen(thinPen);
    painter->setBrush(palette().foreground());
    painter->drawPolygon(QPolygon(3,&triangle[0][0]));
    //圆锥形渐变色中心为（0，0）旋转角为-90度
    QConicalGradient coneGradient(0,0,-90.0);
    coneGradient.setColorAt(0.0,Qt::darkGray);
    coneGradient.setColorAt(0.2,Qt::blue);
    coneGradient.setColorAt(0.5,Qt::white);
    coneGradient.setColorAt(1.0,Qt::darkGray);
    //绘制表盘
    painter->setBrush(coneGradient);
    painter->drawEllipse(-46,-46,92,92);
    //辐射形渐变色中心为（0,0,） 半径20 焦点（0,0,）
    QRadialGradient radialGradient(0,0,20,0,0);
    radialGradient.setColorAt(0.0, Qt::lightGray);
    radialGradient.setColorAt(0.8,Qt::darkGray);
    radialGradient.setColorAt(0.9,Qt::white);
    radialGradient.setColorAt(1.0,Qt::black);
    //绘制内盘
    painter->setPen(Qt::NoPen);
    painter->setBrush(radialGradient);
    painter->drawEllipse(-20,-20,40,40);
    //线性渐变色
    QLinearGradient linearGradient(-7, -25 ,7 ,-25);
    linearGradient.setColorAt(0.0, Qt::black);
    linearGradient.setColorAt(0.3, Qt::lightGray);
    linearGradient.setColorAt(0.8, Qt::white);
    linearGradient.setColorAt(1.0, Qt::black);
    //绘制指针
    painter->rotate(getTimer() * DegressPerSecond);
    painter->setBrush(linearGradient);
    painter->setPen(thinPen);
    painter->drawRoundRect(-7,-25,14,50,170,150);
    //绘制表盘刻度
    for (int i = 0; i<=MaxMinutes-1; ++i) {  //MaxMinutes-1防止60与0重合
        if( i % 5 == 0) {
            painter->setPen(thickPen);
            painter->drawLine(0,-41,0,-44);
            painter->drawText(-15,-41,30,30,Qt::AlignHCenter | Qt::AlignTop,QString::number(i));
        } else {
            painter->setPen(thinPen);
            painter->drawLine(0,-42,0,-44);
        }
        painter->rotate(-DegressPerMinute);
    }
}
```
界面截图如下：

![image](/images/uploads/2009/07/171212076.p.png?d=20090705172314576)
---

PS:最近电脑经常出问题，看来是RP太差了....
