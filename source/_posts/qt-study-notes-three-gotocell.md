---
layout: post
title: 'QT学习笔记之三 GoToCell'
date: 2009-4-14
wordpress_id: 276
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
先用Qt Designer设计，窗体的基本框架，然后进行编译 ，以下是相关代码：
gotocell.h

``` cpp
#ifndef GOTOCELL_H
#define GOTOCELL_H
#include <QtGui/QDialog>
#include "ui_gotocell.h"
namespace Ui
{
    class goToCellClass;
}
class goToCell : public QDialog,public Ui::goToCellClass   // public Ui::goToCellClass这句话要再Qt Designer设计完后加上不然会编译错误
{
    Q_OBJECT
public:
    goToCell(QWidget *parent = 0);
    ~goToCell();
private slots:
    void on_lineEdit_textChanged();
private:
    Ui::goToCellClass *ui;
};
#endif // GOTOCELL_H
```

gotocell.cpp

``` cpp
#include<QtGui>
#include "gotocell.h"
#include "ui_gotocell.h"
goToCell::goToCell(QWidget *parent)
    : QDialog(parent), ui(new Ui::goToCellClass)
{
    ui->setupUi(this);
    QRegExp regExp("[A-Za-z][1-9][0-9]{0,2}");      //[A-Za-z][1-9][0-9]{0,2}的意思是：允许输入大小写字母，
                                                    //后面跟着一个范围为1-9的数字，后面再跟着0个，1个或者2个的0-9的数字
    lineEdit->setValidator(new QRegExpValidator(regExp,this));   // 添加一个验证器用来验证lineEdit里面的字符串
    connect(okButton,SIGNAL(accepted()),this,SLOT(accept()));
    connect(cancelButton,SIGNAL(rejected()),this,SLOT(reject()));
}
goToCell::~goToCell()
{
    delete ui;
}
void goToCell::on_lineEdit_textChanged()
{
    okButton->setEnabled(lineEdit->hasAcceptableInput());
}
```

main.cpp

``` cpp
#include <QtGui/QApplication>
#include "gotocell.h"
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    goToCell *dialog=new goToCell;
    dialog->show();
    return app.exec();
}
```