---
layout: post
title: 'QT学习笔记之十二 ListViewer'
date: 2009-7-18
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
这个主要练习项视图的显示并设置了insert和delete功能

---

**listviewer.h**

``` cpp
#ifndef LISTVIEWER_H
#define LISTVIEWER_H
#include <QtGui/QDialog>
class QDialogButtonBox;
class QListView;
class QStringListModel;
namespace Ui
{
    class ListViewer;
}
class ListViewer : public QDialog
{
    Q_OBJECT
public:
    ListViewer(const QStringList &itemList,QWidget *parent = 0);
    ~ListViewer();
    QStringList itemList() const;
private slots:
    void insert();
    void del();
private:
    Ui::ListViewer *ui;
    QListView *listView;
    QDialogButtonBox *buttonBox;
    QStringListModel *model;
};
#endif // LISTVIEWER_H
```
**listviewer.cpp**

```
#include <QtGui>
#include "listviewer.h"
#include "ui_listviewer.h"
ListViewer::ListViewer(const QStringList &itemList,QWidget *parent)
    : QDialog(parent), ui(new Ui::ListViewer)
{
    ui->setupUi(this);
    //model
    model = new QStringListModel(this);
    model->setStringList(itemList);
    //listView
    listView = new QListView;
    listView->setModel(model);
    listView->setEditTriggers(QAbstractItemView::AnyKeyPressed | QAbstractItemView::DoubleClicked); //The QAbstractItemView class provides the basic functionality for item view classes.
    //buttonBox
    buttonBox = new QDialogButtonBox();
    QPushButton *insertButton = buttonBox->addButton(tr("&Insert"),QDialogButtonBox::ActionRole);  //ActionRole:Clicking the button causes changes to the elements within the dialog
    QPushButton *deleteButton = buttonBox->addButton(tr("&Delete"),QDialogButtonBox::ActionRole);
    buttonBox->addButton(QDialogButtonBox::Ok);
    buttonBox->addButton(QDialogButtonBox::Cancel);
    //signals and slots
    connect(insertButton,SIGNAL(clicked()),this,SLOT(insert()));
    connect(deleteButton,SIGNAL(clicked()),this,SLOT(del()));
    connect(buttonBox,SIGNAL(accepted()),this,SLOT(accept()));
    connect(buttonBox,SIGNAL(rejected()),this,SLOT(reject()));
    //layout
    QVBoxLayout *layout = new QVBoxLayout;
    layout->addWidget(listView);
    layout->addWidget(buttonBox);
    setLayout(layout);
}
ListViewer::~ListViewer()
{
    delete ui;
}
QStringList ListViewer::itemList() const
{
    return model->stringList();
}
void ListViewer::insert()
{
    int row = listView->currentIndex().row();
    model->insertRows(row, 1);
    QModelIndex index=model->index(row);  //This class is used as an index into item models derived from QAbstractItemModel.
                                          //The index is used by item views, delegates, and selection models to locate an item in the model.
    listView->setCurrentIndex(index);
    listView->edit(index);
}
void ListViewer::del()
{
    model->removeRows(listView->currentIndex().row(),1);
}
```
