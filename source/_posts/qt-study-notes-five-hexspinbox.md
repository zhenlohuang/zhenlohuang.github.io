---
layout: post
title: 'QT学习笔记之五 HexSpinBox'
date: 2009-4-28
wordpress_id: 285
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
这个程序主要为了，练习自定义Qt窗体部件，HexSpinBox实现了一个16进制的SpinBox，所以只要重载SpinBox就可以了...详见代码

==================================================================

hexspinbox.h

``` cpp
#ifndef HEXSPINBOX_H
#define HEXSPINBOX_H
#include <QtGui/QWidget>
#include <QSpinBox>
namespace Ui
{
    class HexSpinBoxClass;
}
class HexSpinBox : public QSpinBox     //继承QSpinBox 控件
{
    Q_OBJECT
public:
    HexSpinBox(QWidget *parent = 0);
    ~HexSpinBox();
protected:
    QValidator::State validate(QString &text,int &pos) const;
    int valueFromText(const QString &text) const;   //virtual函数必须重载
    QString textFromValue(int value) const;         //virtual函数必须重载
private:
    Ui::HexSpinBoxClass *ui;
    QRegExpValidator *validator;
};
#endif // HEXSPINBOX_H
```

hexspinbox.cpp

``` cpp
#include "hexspinbox.h"
#include "ui_hexspinbox.h"
HexSpinBox::HexSpinBox(QWidget *parent)
        :QSpinBox(parent)
{
    ui->setupUi(this);
    setRange(0,255);
    validator=new QRegExpValidator(QRegExp("[0-9A-Fa-f]{1,8}"),this);
}
HexSpinBox::~HexSpinBox()
{
    delete ui;
}
/*检查用户输入的合法性*/
QValidator::State HexSpinBox::validate(QString &text,int &pos) const
{
    return validator->validate(text,pos);
}
/*整型值转换为16进制的字符串*/
QString HexSpinBox::textFromValue(int value) const
{
    return QString::number(value,16).toUpper();
}
/*字符串到整数值，若字符串合法ok返回true，否则返回false*/
int HexSpinBox::valueFromText(const QString &text) const
{
    bool ok;
    return text.toInt(&ok,16);
}
```

main.cpp

``` cpp
#include <QtGui/QApplication>
#include "hexspinbox.h"
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    HexSpinBox *hexSpinBox=new HexSpinBox;
    hexSpinBox->sizeHint();
    hexSpinBox->show();
    return app.exec();
}
```
![image](/images/uploads/2009/04/223451170.p.JPG?d=20090430223817014)

PS:庆祝空间人气突破25000
