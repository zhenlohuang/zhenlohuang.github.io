---
layout: post
title: 'QT学习笔记之二 FindDialog'
date: 2009-4-12
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
创建一个FindDialog，功能还没有添加…以后再搞吧

代码：

findDialog.h

``` cpp
#ifndef FINDDIALOG_H
#define FINDDIALOG_H
#include <QtGui/QDialog>
#include<QCheckBox>
#include<QLabel>
#include<QLineEdit>
#include<QPushButton>
namespace Ui
{
    class FindDialogClass;
}
class FindDialog : public QDialog
{
    Q_OBJECT
public:
    FindDialog(QWidget *parent = 0);
    ~FindDialog();
signals:
    void findNext(const QString &amp;str,Qt::CaseSensitivity cs);  //不要写成 Qt::CaseSensitive,Qt::CaseSensitivity是一个类型
    void findPrev(const QString &amp;str,Qt::CaseSensitivity cs);
private slots:
    void findClicked();
    void enableFindButton(const QString &amp;text);
private:
    Ui::FindDialogClass *ui;
    QLabel *label;
    QLineEdit *lineEdit;
    QCheckBox *caseCheckBox;
    QCheckBox *backwardCheckBox;
    QPushButton *findButton;
    QPushButton *closeButton;
};
#endif // FINDDIALOG_H
```
findDialog.cpp

``` cpp
#include<QtGui>
#include<QHBoxLayout>
#include "finddialog.h"
#include "ui_finddialog.h"
FindDialog::FindDialog(QWidget *parent)
    : QDialog(parent), ui(new Ui::FindDialogClass)
{
    ui->setupUi(this);
    //对部件进行定义
    label=new QLabel(tr("&amp;Input the word you want to find"));
    lineEdit=new QLineEdit();
    label->setBuddy(lineEdit);  //setBuddy 当按下快捷键时，转换到lineEdit
    caseCheckBox=new QCheckBox(tr("Match &amp;Case"));
    backwardCheckBox=new QCheckBox(tr("Search &amp;backward"));
    findButton=new QPushButton(tr("Find"));
    closeButton=new QPushButton(tr("Close"));
    //添加信号连接
    connect(lineEdit,SIGNAL(textChanged(const QString &amp;)),this,SLOT(enableFindButton(const QString &amp;)));
    connect(findButton,SIGNAL(clicked()),this,SLOT(findClicked()));
    connect(closeButton,SIGNAL(clicked()),this,SLOT(close()));
    //布局控制
    QHBoxLayout *topLeftLayout=new QHBoxLayout;
    topLeftLayout->addWidget(label);
    topLeftLayout->addWidget(lineEdit);
    QVBoxLayout *leftLayout=new QVBoxLayout;
    leftLayout->addLayout(topLeftLayout);   //添加topLeftLayout层到leftLayout层上
    leftLayout->addWidget(caseCheckBox);
    leftLayout->addWidget(backwardCheckBox);
    QVBoxLayout *rightLayout=new QVBoxLayout;
    rightLayout->addWidget(findButton);
    rightLayout->addWidget(closeButton);
    rightLayout->addStretch();   //添加延展
    //设置mianLayout将leftLayout和rightLayout添加到上面
    QHBoxLayout *mainLayout=new QHBoxLayout;
    mainLayout->addLayout(leftLayout);
    mainLayout->addLayout(rightLayout);
    this->setLayout(mainLayout);
    this->setWindowTitle(tr("Find"));
    this->setFixedHeight(sizeHint().height());
}
FindDialog::~FindDialog()
{
    delete ui;
}
void FindDialog::findClicked()
{
    QString text=lineEdit->text();
    Qt::CaseSensitivity cs=caseCheckBox->isChecked()?Qt::CaseSensitive : Qt::CaseInsensitive;
    if(backwardCheckBox->isChecked())
        emit findPrev(text,cs);     //emit用于发射信号
    else emit findNext(text,cs);
}
void FindDialog::enableFindButton(const QString &amp;text)
{
    findButton->setEnabled(!text.isEmpty());
}
```

main.cpp

``` cpp
#include <QtGui/QApplication>
#include "finddialog.h"
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    FindDialog *findDialog=new FindDialog;
    findDialog->show();
    return app.exec();
}
```
运行结果：

![image](/images/legacy/2009/04/223312202.p.jpg)

PS
编译过程出现这个错误：
:-1: error: collect2: ld returned 1 exit status
，但是当我程序写完的时候问题就解决了
….orz，顺便说下，QtCreator的补全功能还是很不错的….1.0果然比0.9好太多了….赞一个