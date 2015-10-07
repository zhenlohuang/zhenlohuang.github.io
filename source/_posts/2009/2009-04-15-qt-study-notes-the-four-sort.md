---
layout: post
title: 'QT学习笔记之四 Sort'
date: 2009-4-15
wordpress_id: 277
permalink: /archives/qt-study-notes-the-four-sort.html
categories: [Programming]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
这几次主要练习Qt Designer的设计，所以就做了对话框，没有去做具体的实现，以后有空再写吧….哇哈哈

主要代码如下：

sortdialog.h

``` cpp
#ifndef SORTDAILOG_H
#define SORTDAILOG_H
#include <QtGui/QDialog>
#include "ui_sortdailog.h"
namespace Ui
{
    class sortdailogClass;
}
class sortdailog : public QDialog
{
    Q_OBJECT
public:
    sortdailog(QWidget *parent = 0);
    ~sortdailog();
    void setColumnRange(QChar first,QChar last);  //用来设置列的范围
private:
    Ui::sortdailogClass *ui;
};
#endif // SORTDAILOG_H
```
sortdialog.cpp

``` cpp
#include "sortdailog.h"
#include<QtGui>
 
sortdailog::sortdailog(QWidget *parent)
    : QDialog(parent)
{
    ui->setupUi(this);
    ui->secondaryGroupBox->hide();  //设置为隐藏
    ui->tertiaryGroupBox->hide();    //设置为隐藏
    this->layout()->setSizeConstraint(QLayout::SetFixedSize);  //将此层设置为适合大小
    setColumnRange('A','Z');  //设置默认范围*/
}
sortdailog::~sortdailog()
{
    delete ui;
}
void sortdailog::setColumnRange(QChar first,QChar last)
{
    //清除原有数据
    ui->primaryComboBox->clear();
    ui->secondaryComboBox->clear();
    ui->tertiaryComboBox->clear();
    ui->secondaryComboBox->addItem(tr("None"));
    ui->tertiaryComboBox->addItem(tr("None"));
    ui->primaryComboBox->setMinimumSize(ui->secondaryComboBox->sizeHint());  //设置理想大小
    QChar ch=first;
    while(ch<=last)
    {
        ui->primaryComboBox->addItem(QString(ch));  //QString(ch)因为tr()只能接受字符串类型
        ui->secondaryComboBox->addItem(QString(ch));
        ui->tertiaryComboBox->addItem(QString(ch));
        ch=ch.unicode()+1;
    }
}
```

main.cpp

``` cpp
#include <QtGui/QApplication>
#include "sortdailog.h"
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    sortdailog *dialog=new sortdailog;
    dialog->setColumnRange('C','H');
    dialog->show();
    return a.exec();
}
```