---
layout: post
title: 'QT学习笔记之七 FindFileDialog'
date: 2009-5-29
wordpress_id: 304
permalink: /archives/qt-study-notes-the-seven-findfiledialog.html
categories: [Programming]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
好久没写，最近东搞搞西搞搞都没怎么看Qt，惭愧......

这里主要练习Qt控件的布局问题，用了三种布局方式写了个FindFile

操作系统：Ubuntu 9.04

Qt版本：4.5

FindFile1

``` cpp
#ifndef FINDFILE_H
#define FINDFILE_H
#include <QtGui/QWidget>
class QCheckBox;
class QLabel;
class QLineEdit;
class QPushButton;
class QTableWidget;
namespace Ui
{
    class findfile;
}
class findfile : public QWidget
{
    Q_OBJECT
public:
    findfile(QWidget *parent = 0);
    ~findfile();
private:
    Ui::findfile *ui;
    QLabel *namedLabel;
    QLabel *lookInLabel;
    QLineEdit *namedLineEdit;
    QLineEdit *lookInLineEdit;
    QCheckBox *subfoldersCheckBox;
    QTableWidget *tableWidget;
    QLabel *messageLabel;
    QPushButton *findButton;
    QPushButton *stopButton;
    QPushButton *closeButton;
    QPushButton *helpButton;
};
#endif // FINDFILE_H
```

``` cpp
#include <QtGui>
#include "findfile.h"
#include "ui_findfile.h"
findfile::findfile(QWidget *parent)
    : QWidget(parent), ui(new Ui::findfile)
{
    ui->setupUi(this);
    namedLabel = new QLabel(tr("&amp;Named:"),this);
    namedLineEdit = new QLineEdit(this);
    namedLabel->setBuddy(namedLineEdit);  //Buddy ֵ
    lookInLabel = new QLabel(tr("&amp;Look in:"),this);
    lookInLineEdit = new QLineEdit(this);
    lookInLabel->setBuddy(lookInLineEdit);
    subfoldersCheckBox = new QCheckBox(tr("Include subfolders"),this);
    QStringList labels;
    labels << tr("Name") <<tr("In Folder") <<tr("Size")
           << tr("Modified");  //QStringList add string
    tableWidget = new QTableWidget(this);
    tableWidget->setColumnCount(4);
    tableWidget->setHorizontalHeaderLabels(labels);  //ñֶ
    messageLabel = new QLabel(tr("0 files found"),this);
    messageLabel->setFrameShape(QLabel::Panel);  //Panel
    messageLabel->setFrameShadow(QLabel::Sunken);  //Sunken
    findButton = new QPushButton(tr("&amp;Find"),this);
    stopButton = new QPushButton(tr("&amp;Stop"),this);
    closeButton =new QPushButton(tr("&amp;Close"),this);
    helpButton =new QPushButton(tr("&amp;Help"),this);
    connect(closeButton,SIGNAL(clicked()),this,SLOT(close()));
    //ؼ
    /*
        void setGeometry ( int x, int y, int w, int h )
        void setGeometry ( const QRect &amp; )
      */
    namedLabel->setGeometry(9, 9, 50, 25);
    namedLineEdit->setGeometry(65, 9, 200, 25);
    lookInLabel->setGeometry(9, 40, 50, 25);
    lookInLineEdit->setGeometry(65, 40, 200, 25);
    subfoldersCheckBox->setGeometry(9, 71, 256, 23);
    tableWidget->setGeometry(9, 100, 256, 100);
    messageLabel->setGeometry(9, 206, 256, 25);
    findButton->setGeometry(271, 9, 85, 32);
    stopButton->setGeometry(271, 47, 85, 32);
    closeButton->setGeometry(271, 84, 85, 32);
    helpButton->setGeometry(271, 199, 85, 32);
    setWindowTitle(tr("Find files or folders"));
    setFixedSize(365,240);
}
findfile::~findfile()
{
    delete ui
}
```
FindFile2

``` cpp
#ifndef FINDFILEDIALOG_H
#define FINDFILEDIALOG_H
#include <QtGui/QDialog>
class QCheckBox;
class QLabel;
class QLineEdit;
class QPushButton;
class QTableWidget;
namespace Ui
{
    class FindFileDialog;
}
class FindFileDialog : public QDialog
{
    Q_OBJECT
public:
    FindFileDialog(QWidget *parent = 0);
    ~FindFileDialog();
protected:
    void resizeEvent(QResizeEvent *);
private:
    Ui::FindFileDialog *ui;
    QLabel *namedLabel;
    QLabel *lookInLabel;
    QLineEdit *lookInLineEdit;
    QLineEdit *namedLineEdit;
    QCheckBox *subfoldersCheckBox;
    QTableWidget *tableWidget;
    QLabel *messageLabel;
    QPushButton *findButton;
    QPushButton *stopButton;
    QPushButton *closeButton;
    QPushButton *helpButton;
};
#endif // FINDFILEDIALOG_H
```

``` cpp
#include<QtGui>
#include "findfiledialog.h"
#include "ui_findfiledialog.h"
FindFileDialog::FindFileDialog(QWidget *parent)
    : QDialog(parent), ui(new Ui::FindFileDialog)
{
    ui->setupUi(this);
    namedLabel = new QLabel(tr("&amp;Named:"),this);
    namedLineEdit = new QLineEdit(this);
    namedLabel->setBuddy(namedLineEdit);
    lookInLabel = new QLabel(tr("&amp;name"),this);
    lookInLineEdit = new QLineEdit(this);
    lookInLabel->setBuddy(lookInLineEdit);
    subfoldersCheckBox = new QCheckBox(tr("Include subfolders"),this);
    QStringList labels;
    labels << tr("Name") << tr("In Folder") << tr("Size") <<tr("Modified");
    tableWidget = new QTableWidget(this);
    tableWidget->setColumnCount(4);
    tableWidget->setHorizontalHeaderLabels(labels);
    messageLabel = new QLabel(tr("0 files found"),this);
    messageLabel->setFrameShape(QLabel::Panel);
    messageLabel->setFrameShadow(QLabel::Sunken);
    findButton = new QPushButton(tr("&amp;Find"),this);
    stopButton = new QPushButton(tr("Stop"),this);
    closeButton = new QPushButton(tr("Close"),this);
    helpButton = new QPushButton(tr("Help"),this);
    connect(closeButton,SIGNAL(clicked()),this,SLOT(close()));
    setWindowTitle(tr("Find Files or Folders"));
    setMinimumSize(265,190);
    resize(365,240);
}
FindFileDialog::~FindFileDialog()
{
    delete ui;
}
void FindFileDialog::resizeEvent(QResizeEvent *)
{
    int extraWidth = width()- minimumWidth();
    int extraHeight = height() - minimumHeight();
    namedLabel->setGeometry(9,9,50,25);
    namedLineEdit->setGeometry(65,9,100+extraWidth,25);
    lookInLabel->setGeometry(9,40,50,25);
    lookInLineEdit->setGeometry(65,40,100+extraWidth,25);
    subfoldersCheckBox->setGeometry(9,71,156+extraWidth,23);
    tableWidget->setGeometry(9,100,156 + extraWidth,50 + extraHeight);
    messageLabel->setGeometry(9,156 + extraHeight,156 + extraWidth,25);
    findButton->setGeometry(171 + extraWidth,9,85,32);
    stopButton->setGeometry(171 + extraWidth,47,85,32);
    closeButton->setGeometry(171 + extraWidth,84,85,32);
    helpButton->setGeometry(171 + extraWidth,149 + extraHeight,85,32);
}
```
FindFile3

``` cpp
#ifndef FINDFILEDIALOG_H
#define FINDFILEDIALOG_H
#include <QtGui/QDialog>
class QCheckBox;
class QLabel;
class QLineEdit;
class QPushButton;
class QTableWidget;
namespace Ui
{
    class FindFileDialog;
}
class FindFileDialog : public QDialog
{
    Q_OBJECT
public:
    FindFileDialog(QWidget *parent = 0);
    ~FindFileDialog();
private:
    Ui::FindFileDialog *ui;
    QLabel *namedLabel;
    QLabel *lookInLabel;
    QLineEdit *lookInLineEdit;
    QLineEdit *namedLineEdit;
    QCheckBox *subfoldersCheckBox;
    QTableWidget *tableWidget;
    QLabel *messageLabel;
    QPushButton *findButton;
    QPushButton *stopButton;
    QPushButton *closeButton;
    QPushButton *helpButton;
};
#endif // FINDFILEDIALOG_H
```

``` cpp
#include <QtGui>
#include "findfiledialog.h"
#include "ui_findfiledialog.h"
FindFileDialog::FindFileDialog(QWidget *parent)
    : QDialog(parent), ui(new Ui::FindFileDialog)
{
    ui->setupUi(this);
    namedLabel = new QLabel(tr("&amp;Named:"));
    namedLineEdit = new QLineEdit;
    namedLabel->setBuddy(namedLineEdit);
    lookInLabel = new QLabel(tr("&amp;Look in:"));
    lookInLineEdit = new QLineEdit;
    lookInLabel->setBuddy(lookInLineEdit);
    subfoldersCheckBox = new QCheckBox(tr("Include subfolders"));
    QStringList labels;
    labels << tr("Name") << tr("In Folder") << tr("Size")
           << tr("Modified");
    tableWidget = new QTableWidget;
    tableWidget->setColumnCount(4);
    tableWidget->setHorizontalHeaderLabels(labels);
    messageLabel = new QLabel(tr("0 files found"));
    messageLabel->setFrameShape(QLabel::Panel);
    messageLabel->setFrameShadow(QLabel::Sunken);
    findButton = new QPushButton(tr("&amp;Find"));
    stopButton = new QPushButton(tr("Stop"));
    closeButton = new QPushButton(tr("Close"));
    helpButton = new QPushButton(tr("Help"));
    connect(closeButton, SIGNAL(clicked()), this, SLOT(close()));
    QGridLayout *leftLayout = new QGridLayout;    //网格布局
    leftLayout->addWidget(namedLabel,0,0);
    leftLayout->addWidget(namedLineEdit,0,1);
    leftLayout->addWidget(lookInLabel,1,0);
    leftLayout->addWidget(lookInLineEdit,1,1);
    leftLayout->addWidget(subfoldersCheckBox,2,0,1,2);
    leftLayout->addWidget(tableWidget,3,0,1,2);
    leftLayout->addWidget(messageLabel,4,0,1,2);
    QVBoxLayout *rightLayout = new QVBoxLayout;
    rightLayout->addWidget(findButton);
    rightLayout->addWidget(stopButton);
    rightLayout->addWidget(closeButton);
    rightLayout->addStretch();
    rightLayout->addWidget(helpButton);
    QHBoxLayout *mainLayout = new QHBoxLayout;
    mainLayout->addLayout(leftLayout);
    mainLayout->addLayout(rightLayout);
    setLayout(mainLayout);   //设置主层
    setWindowTitle(tr("Find Files or Folders"));
}
FindFileDialog::~FindFileDialog()
{
    delete ui;
}
```
![image](/images/uploads/2009/05/123205680.p.png?d=20090529123218071)

PS:个人感觉第三种比较和谐..........

---

一个控件写了三种，我果然很闲............
