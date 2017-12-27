---
layout: post
title: 'QT学习笔记之十四 StringMatch'
date: 2009-7-18
wordpress_id: 329
categories: [Programming]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---

用于正则表达式，通配符，完整匹配三种方式

**stringmatch.h**

``` cpp
#ifndef STRINGMATCH_H
#define STRINGMATCH_H
#include <QtGui/QDialog>
class QComboBox;
class QLabel;
class QLineEdit;
class QListView;
class QSortFilterProxyModel;
class QStringListModel;
namespace Ui
{
    class StringMatch;
}
class StringMatch : public QDialog
{
    Q_OBJECT
public:
    StringMatch(QWidget *parent = 0);
    ~StringMatch();
private slots:
    void reapplyFilter();
private:
    Ui::StringMatch *ui;
    QStringListModel *sourceModel;
    QSortFilterProxyModel *proxyModel;
    QListView *listView;
    QLabel *filterLabel;
    QLabel *syntaxLabel;
    QLineEdit *filterLineEdit;
    QComboBox *syntaxComboBox;
};
#endif // STRINGMATCH_H
```

**stringmatch.cpp**

``` cpp
#include <QtGui>
#include "stringmatch.h"
#include "ui_stringmatch.h"
StringMatch::StringMatch(QWidget *parent)
    : QDialog(parent), ui(new Ui::StringMatch)
{
    ui->setupUi(this);
    QTextCodec::setCodecForCStrings(QTextCodec::codecForLocale());
    QTextCodec::setCodecForTr(QTextCodec::codecForName("utf8"));
    //sourceModel
    sourceModel = new QStringListModel(this);
    sourceModel->setStringList(QColor::colorNames());
    //proxyModel
    proxyModel = new QSortFilterProxyModel(this);
    proxyModel->setSourceModel(sourceModel);
    proxyModel->setFilterKeyColumn(0);
    //listView
    listView = new QListView;
    listView->setModel(proxyModel);
    listView->setEditTriggers(QAbstractItemView::NoEditTriggers);
    //filter
    filterLabel = new QLabel(tr("要匹配的字符串"));
    filterLineEdit = new QLineEdit;
    filterLabel->setBuddy(filterLineEdit);
    //syntax
    syntaxLabel = new QLabel("匹配方式选择");
    syntaxComboBox = new QComboBox;
    syntaxComboBox->addItem("正则表达式",QRegExp::RegExp);   //A rich Perl-like pattern matching syntax. This is the default.
    syntaxComboBox->addItem("通配符",QRegExp::Wildcard);   //This provides a simple pattern matching syntax similar to that used by shells (command interpreters) for "file globbing".
    syntaxComboBox->addItem("完全匹配",QRegExp::FixedString);   //This is equivalent to using the RegExp pattern on a string in which all metacharacters are escaped using
    //当filterLineEdit和syntaxComboBox
    connect(filterLineEdit,SIGNAL(textChanged(const QString &)),this,SLOT(reapplyFilter()));
    connect(syntaxComboBox,SIGNAL(currentIndexChanged(int)),this,SLOT(reapplyFilter()));
    //布局
    QGridLayout *layout = new QGridLayout;
    layout->addWidget(listView,0,0,1,2);
    layout->addWidget(filterLabel,1,0);
    layout->addWidget(filterLineEdit,1,1);
    layout->addWidget(syntaxLabel,2,0);
    layout->addWidget(syntaxComboBox,2,1);
    setLayout(layout);
    setWindowTitle(tr("String Matching"));
}
StringMatch::~StringMatch()
{
    delete ui;
}
void StringMatch::reapplyFilter()
{
    QRegExp::PatternSyntax syntax = QRegExp::PatternSyntax(syntaxComboBox->itemData(
                    syntaxComboBox->currentIndex()).toInt());
    QRegExp regExp(filterLineEdit->text(),Qt::CaseInsensitive,syntax);
    proxyModel->setFilterRegExp(regExp);
}
```

**软件截图：**
![image](/images/uploads/2009/07/233505564.p.png?d=20090718235822986)

---
直到今天才把Qt4的中文乱码搞定太尴尬了.......鄙视下自己.....
只要程序中加入
QTextCodec::setCodecForCStrings(QTextCodec::codecForLocale());
    QTextCodec::setCodecForTr(QTextCodec::codecForName("utf8"));</textarea>

当然要看情况来说，你的环境可能是GBK或者其他的...
