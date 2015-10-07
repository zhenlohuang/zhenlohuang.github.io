---
layout: post
title: 'QT学习笔记之十七  StudentManage'
date: 2009-7-29
wordpress_id: 334
permalink: /archives/qt-study-notes-of-seventeen-studentmanage.html
categories: [Programming]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---

这里利用Qt4+MySQL做一个简单的例子

不会连接MySQL的，参考这里：http://blog.csdn.net/killua_hzl/archive/2009/07/29/4391956.aspx

代码如下：

**studentmanage.h**

``` cpp
#ifndef STUDENTMANAGE_H
#define STUDENTMANAGE_H
#include <QtGui/QWidget>
class QSqlTableModel;
class QTableView;
enum { Stu_Sno = 0,Stu_Name = 1,Stu_Age = 2,Stu_Math = 3,Stu_Chinese = 4,Stu_English = 5 };
class StudentManage : public QWidget
{
    Q_OBJECT
public:
    StudentManage(QWidget *parent = 0);
    ~StudentManage();
private:
    QSqlTableModel *model;
    QTableView *view;
};
#endif // STUDENTMANAGE_H
```

**studentmanage.cpp**

``` cpp
#include <QtGui>
#include <QtSql>
#include "studentmanage.h"
StudentManage::StudentManage(QWidget *parent)
    : QWidget(parent)
{
    model = new QSqlTableModel(this);
    model->setTable("student");
    model->setSort(Stu_Sno, Qt::AscendingOrder);
    model->setHeaderData(Stu_Name, Qt::Horizontal, tr("Name"));
    model->setHeaderData(Stu_Age, Qt::Horizontal, tr("Age"));
    model->setHeaderData(Stu_Math, Qt::Horizontal, tr("Math"));
    model->setHeaderData(Stu_Chinese, Qt::Horizontal, tr("Chinese"));
    model->setHeaderData(Stu_English, Qt::Horizontal, tr("English"));
    model->select();
    view = new QTableView;
    view->setModel(model);
    view->setSelectionMode(QAbstractItemView::SingleSelection);
    view->setSelectionBehavior(QAbstractItemView::SelectRows);
    view->resizeColumnsToContents();
    view->setEditTriggers(QAbstractItemView::NoEditTriggers);
    QHeaderView *headerView = view->horizontalHeader();
    headerView->setStretchLastSection(true);
    QHBoxLayout *layout = new QHBoxLayout;
    layout->addWidget(view);
    setLayout(layout);
    setWindowTitle(tr("Student Manage"));
}
StudentManage::~StudentManage()
{
}
```

**main.cpp**

``` cpp
#include <QtGui/QApplication>
#include <QtSql>
#include <QMessageBox>
#include "studentmanage.h"
/**
  建立数据库链接
  */
bool createConnection()
{
    QSqlDatabase db = QSqlDatabase::addDatabase("QMYSQL");
    db.setHostName("localhost");
    db.setDatabaseName("DB_Stu");
    db.setUserName("root");
    db.setPassword("123456");
    if (!db.open()) {
        QMessageBox::warning(0, QObject::tr("Database Error"),
                             db.lastError().text());
        return false;
    }
    return true;
}
/**
  创建数据
  */
void createData()
{
    QSqlQuery query;
    query.exec("Drop table student");  //如果表存在删除
    //建表
    query.exec("Create table student ("
               "Sno int not null,"
               "Name varchar(40) not null,"
               "Age int not null,"
               "Math int not null,"
               "Chinese int not null,"
               "English int not null)");
    //插入数据
    query.exec("Insert into student (Sno,Name,Age,Math,Chinese,English) values(1,'Dear_fox',1,100,100,100)");
    query.exec("Insert into student (Sno,Name,Age,Math,Chinese,English) values(2,'Killua',2,99,98,98)");
}
/**
  主函数
  */
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    bool isCreate = !QFile::exists("DB_Stu");
    if (!createConnection())
        return 1;
    if(isCreate)
        createData();
    StudentManage studentManage;
    studentManage.resize(400,300);
    studentManage.show();
    return a.exec();
}
```

**结果截图：**

![image](/images/uploads/2009/07/204057561.p.png?d=20090729211407405")
