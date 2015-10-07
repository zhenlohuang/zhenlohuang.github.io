---
layout: post
title: 'QT学习笔记之十一 SymbolPicker'
date: 2009-7-18
wordpress_id: 326
permalink: /archives/qt-study-notes-the-eleven-symbolpicker.html
categories: [Programming]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
最近太忙了，都没怎么看Qt鄙视下自己....

不说了，放代码

**symbolpicker.h**

``` cpp
#ifndef SYMBOLPICKER_H
#define SYMBOLPICKER_H
#include <QtGui/QDialog>
#include <QMap>
class QDialogButtonBox;
class QIcon;
class QListWidget;
namespace Ui
{
    class SymbolPicker;
}
class SymbolPicker : public QDialog
{
    Q_OBJECT
public:
    SymbolPicker(const QMap<int,QString> &symbolMap,QWidget *parent = 0);
    ~SymbolPicker();
    int selected() const {return id;}
    void done(int result);
private:
    Ui::SymbolPicker *ui;
    QIcon iconForSymbol(const QString &symbolName);
    QListWidget *listWidget;
    QDialogButtonBox *buttonBox;
    int id;
};
#endif // SYMBOLPICKER_H
```
**symbolpicker.cpp**

``` cpp
#include<QtGui>
#include "symbolpicker.h"
#include "ui_symbolpicker.h"
SymbolPicker::SymbolPicker(const QMap<int,QString> &symbolMap,QWidget *parent)
    : QDialog(parent), ui(new Ui::SymbolPicker)
{
    ui->setupUi(this);
    id = -1; //设置id默认值
    listWidget = new QListWidget;
    listWidget->setIconSize(QSize(60,60));
    QMapIterator<int,QString> i(symbolMap);
    while(i.hasNext()) {
        i.next();
        QListWidgetItem *item = new QListWidgetItem(i.value(),listWidget);
        item->setIcon(iconForSymbol(i.value()));
        item->setData(Qt::UserRole,i.key());
    }
   buttonBox = new QDialogButtonBox(QDialogButtonBox::Ok| QDialogButtonBox::Cancel);
    connect(buttonBox,SIGNAL(accepted()),this,SLOT(accept()));
    connect(buttonBox,SIGNAL(rejected()),this,SLOT(reject()));
    QVBoxLayout *layout = new QVBoxLayout;
    layout->addWidget(listWidget);
    layout->addWidget(buttonBox);
    setLayout(layout);
    setWindowTitle(tr("Symbol Picker"));
}
SymbolPicker::~SymbolPicker()
{
    delete ui;
}
void SymbolPicker::done(int result)
{
    id = -1;
    if (result == QDialog::Accepted) {
        QListWidgetItem *item = listWidget->currentItem();
        if (item)
            id = item->data(Qt::UserRole).toInt();
    }
    QDialog::done(result);
}
QIcon SymbolPicker::iconForSymbol(const QString &symbolName)
{
    QString fileName = ":/images/" + symbolName.toLower();
    fileName.replace(' ','-');
    return QIcon(fileName);
}
```
**symbolpicker.qrc (资源文件)**

``` xml
<RCC>
<qresource>
    <file>images/killua.png</file>
</qresource>
</RCC>
```

刚开始icon显示一直有问题，后来配置了下资源文件就OK了...