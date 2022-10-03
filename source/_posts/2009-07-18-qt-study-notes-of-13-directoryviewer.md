---
layout: post
title: 'QT学习笔记之十三 DirectoryViewer'
date: 2009-7-18
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
这个还是练习项视图，用于浏览目录，可以添加新文件夹和删除文件或文件夹

**directoryviewer.h**

```
#ifndef DIRECTORYVIEWER_H
#define DIRECTORYVIEWER_H
#include <QtGui/QDialog>
class QDialogButtonBox;
class QDirModel;
class QTreeView;
namespace Ui
{
    class DirectoryViewer;
}
class DirectoryViewer : public QDialog
{
    Q_OBJECT
public:
    DirectoryViewer(QWidget *parent = 0);
    ~DirectoryViewer();
private slots:
    void createDirectory();
    void remove();
private:
    Ui::DirectoryViewer *ui;
    QTreeView *treeView;
    QDirModel *model;
    QDialogButtonBox *buttonBox;
};
#endif // DIRECTORYVIEWER_H
```
**directoryviewer.cpp**

``` cpp
#include <QtGui>
#include "directoryviewer.h"
#include "ui_directoryviewer.h"
/**
  构造函数
  */
DirectoryViewer::DirectoryViewer(QWidget *parent)
    : QDialog(parent), ui(new Ui::DirectoryViewer)
{
    ui->setupUi(this);
    //model
    model = new QDirModel;
    model->setReadOnly(false);
    model->setSorting(QDir::DirsFirst | QDir::IgnoreCase | QDir::Name);
    //treeView
    treeView = new QTreeView;
    treeView->setModel(model);
    treeView->header()->setStretchLastSection(true);
    treeView->header()->setSortIndicator(0,Qt::AscendingOrder);
    treeView->header()->setSortIndicatorShown(true);
    treeView->header()->setClickable(true);
    QModelIndex index = model->index(QDir::currentPath());
    treeView->expand(index);
    treeView->scrollTo(index);
    treeView->resizeColumnToContents(0);
    //ButtonBox
    buttonBox = new QDialogButtonBox(Qt::Horizontal);
    QPushButton *mkdirButton = buttonBox->addButton(tr("&Create Directory"),QDialogButtonBox::ActionRole);
    QPushButton *removeButton = buttonBox->addButton(tr("&Remove"),QDialogButtonBox::ActionRole);
    buttonBox->addButton(tr("&Quit"),QDialogButtonBox::AcceptRole);

//signal and slot
connect(mkdirButton,SIGNAL(clicked()),this,SLOT(createDirectory()));
connect(removeButton,SIGNAL(clicked()),this,SLOT(remove()));
connect(buttonBox,SIGNAL(accepted()),this,SLOT(accept()));
//Layout
QVBoxLayout *layout = new QVBoxLayout;
layout->addWidget(treeView);
layout->addWidget(buttonBox);
setLayout(layout);
setWindowTitle(tr("Directory Viewer by Killua"));
}
/**
析构函数
*/
DirectoryViewer::~DirectoryViewer()
{
delete ui;
}
/**
文件夹创建
*/
void DirectoryViewer::createDirectory()
{
QModelIndex index = treeView->currentIndex();
if (!index.isValid())
return ;
QString dirName = QInputDialog::getText(this,tr("Create Directory"),tr("Directory Name"));
if (!dirName.isEmpty()) {
if (!model->mkdir(index,dirName).isValid())
QMessageBox::information(this,tr("Create Directory"),tr("Failed to create"));
}
}
/**
删除
*/
void DirectoryViewer::remove()
{
QModelIndex index =treeView->currentIndex();
if (!index.isValid())
return ;
bool ok;
if (model->fileInfo(index).isDir()) {
ok = model->rmdir(index);
} else {
ok = model->remove(index);
}
if (!ok)
QMessageBox::information(this,tr("Remove"),tr("Failed to remvoe"));
}
```

**软件截图：**
![image](/images/legacy/2009/07/233502705.p.png)
