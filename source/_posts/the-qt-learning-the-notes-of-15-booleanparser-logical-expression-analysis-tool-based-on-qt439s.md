---
layout: post
title: 'QT学习笔记之十五 BooleanParser  基于Qt4的逻辑表达式分析工具'
date: 2009-7-29
wordpress_id: 331
categories: [Programming]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
貌似好久没写了，最近比较忙的说，这个主要练习Model的设计，部分代码是参考别人的，大牛们不要鄙视啊....

源码最后附上。


部分核心代码如下：

**booleanmodel.h**

``` cpp
#ifndef BOOLEANMODEL_H
#define BOOLEANMODEL_H
#include <QAbstractItemModel>
class Node;
class BooleanModel : public QAbstractItemModel
{
    Q_OBJECT
public:
    BooleanModel(QObject *parent = 0);
    ~BooleanModel();
    void setRootNode(Node *node);
    QModelIndex index(int row, int column, const QModelIndex &parent) const;
    QModelIndex parent(const QModelIndex &child) const;
    int rowCount(const QModelIndex &parent) const;
    int columnCount(const QModelIndex &parent) const;
    QVariant data(const QModelIndex &index, int role) const;
    QVariant headerData(int section,Qt::Orientation orientation, int role) const;
private:
    Node *nodeFromIndex(const QModelIndex &index) const;
    Node *rootNode;
};
#endif // BOOLEANMODEL_H
```

**booleanmodel.cpp**

``` cpp
#include <QtCore>
#include "booleanmodel.h"
#include "booleanparser.h"
/**
  构造函数，初始化根结点为空
  */
BooleanModel::BooleanModel(QObject *parent) :QAbstractItemModel(parent)
{
    rootNode = 0;
}
/**
  析构函数，删除根结点
  */
BooleanModel::~BooleanModel()
{
    delete rootNode;
}
/**
  设置根结点,删除原来根结点，重新设置根结点
  */
void BooleanModel::setRootNode(Node *node)
{
    delete rootNode;
    rootNode = node;
    reset();  //刷新表格数据
}
/**
  返回索引，这个是virtual函数所以一定要重载
  */
QModelIndex BooleanModel::index(int row,int column,const QModelIndex &parent) const
{
    if (!rootNode || row < 0 || column < 0)
        return QModelIndex();    //非法情况
    Node *parentNode = nodeFromIndex(parent);
    Node *childNode = parentNode->children.value(row);
    if (!childNode)
        return QModelIndex();
    return createIndex(row,column, childNode);
}
/**
  返回父亲结点
  */
QModelIndex BooleanModel::parent(const QModelIndex &child) const
{
    Node *node = nodeFromIndex(child);
    if (!node)
        return QModelIndex(); //结点无效
    Node *parentNode = node->parent;
    if (!parentNode)
        return QModelIndex();  //父结点无效
    Node *grandparentNode = parentNode->parent;
    if(!grandparentNode)
        return QModelIndex();  //父结点的父结点无效
    int row = grandparentNode->children.indexOf(parentNode);
    return createIndex(row,0,parentNode);
}
/**
  返回行数，这个也是virtual函数，要重载
  */
int BooleanModel::rowCount(const QModelIndex &parent) const
{
    if (parent.column() > 0)
        return 0;
    Node *parentNode = nodeFromIndex(parent);
    if(!parentNode)
        return 0;
    return parentNode->children.count();
}
/**
  返回列数，固定只有2，这个也是virtual函数，要重载
  */
int BooleanModel::columnCount(const QModelIndex & /*parent*/) const
{
    return 2;
}
/**
  返回数据，这个也是virtual函数，要重载
  */
QVariant BooleanModel::data(const QModelIndex &index, int role) const
{
    if (role != Qt::DisplayRole)
        return QVariant();    //不是显示角色，认为是无效数据
    Node *node = nodeFromIndex(index);
    if (!node)
        return QVariant();     //结点无效
    if(index.column() == 0) {
        switch (node->type) {
            case Node::Root:
                return tr("Root");
            case Node::OrExpression:
                return tr("OR Expression");
            case Node::AndExpression:
                return tr("AND Expression");
            case Node::NotExpression:
                return tr("NOT Expression");
            case Node::Atom:
                return tr("Atom");
            case Node::Identifier:
                return tr("Identifier");
            case Node::Operator:
                return tr("Operator");
            case Node::Punctuator:
                return tr("Punctuator");
            default:
                return tr("Unknown");    //默认为未知
            }
        } else if(index.column() == 1) {
            return node->str;
        }
        return QVariant();
}
/**
  返回表头
  */
QVariant BooleanModel::headerData(int section, Qt::Orientation orientation, int role) const
{
    if (orientation == Qt::Horizontal && role == Qt::DisplayRole) {
        if (section == 0) {
            return tr("Node");
        } else if (section == 1) {
            return tr("Value");
        }
    }
        return QVariant();
}
/**
  从索引返回结点
  */
Node *BooleanModel::nodeFromIndex(const QModelIndex &index) const
{
    if (index.isValid()) {
        return static_cast<Node *>(index.internalPointer());
    } else {
        return rootNode;
    }
}
```

**booleanparser.h**

``` cpp
#ifndef BOOLEANPARSER_H
#define BOOLEANPARSER_H
#include "node.h"
class BooleanParser
{
public:
    BooleanParser();
    Node *parse(const QString &expr);
private:
    Node *parseOrExpression();
    Node *parseAndExpression();
    Node *parseNotExpression();
    Node *parseAtom();
    Node *parseIdentifier();
    void addChild(Node *parent, Node *child);
    void addToken(Node *parent, const QString &str, Node::Type type);
    bool matchToken(const QString &str) const;
    QString in;
    int pos;
};
#endif // BOOLEANPARSER_H
```

**booleanparser.cpp**

```
#include <QtCore>
#include "booleanparser.h"
#include "node.h"
BooleanParser::BooleanParser()
{
}
Node *BooleanParser::parse(const QString &expr)
{
    in = expr;
    in.replace(" ", "");
    pos = 0;
    Node *node = new Node(Node::Root);
    addChild(node, parseOrExpression());
    return node;
}
/**
  解析或表达式
  */
Node *BooleanParser::parseOrExpression()
{
    Node *childNode = parseAndExpression();
    if (matchToken("||")) {
        Node *node = new Node(Node::OrExpression);
        addChild(node,childNode);
        while (matchToken("||")) {
            addToken(node,"||", Node::Operator);
            addChild(node, parseAndExpression());
        }
        return node;
    } else {
        return childNode;
    }
}
/**
  解析与表达式
  */
Node *BooleanParser::parseAndExpression()
{
    Node *childNode = parseNotExpression();
    if (matchToken("&&")) {
        Node *node = new Node(Node::AndExpression);
        addChild(node, childNode);
        while (matchToken("&&")) {
            addToken(node, "&&", Node::Operator);
            addChild(node, parseNotExpression());
        }
        return node;
    } else {
        return childNode;
    }
}
/**
  解析非表达式
  */
Node *BooleanParser::parseNotExpression()
{
    if (matchToken("!")) {
        Node *node = new Node(Node::NotExpression);
        while (matchToken("!"))
            addToken(node, "!", Node::Operator);
        addChild(node, parseAtom());
        return node;
    } else {
        return parseAtom();
    }
}
/**
  解析一个项
  */
Node *BooleanParser::parseAtom()
{
    if (matchToken("(")) {
        Node *node = new Node(Node::Atom);
        addToken(node, "(", Node::Punctuator);
        addChild(node, parseOrExpression());
        addToken(node, ")", Node::Punctuator);
        return node;
    } else {
        return parseIdentifier();
    }
}
/**
  解析标识符
  */
Node *BooleanParser::parseIdentifier()
{
    int startPos = pos;
    while (pos < in.length() && in[pos].isLetterOrNumber())
        ++pos;
    if (pos > startPos) {
        return new Node(Node::Identifier,
                        in.mid(startPos, pos - startPos));
    } else {
        return 0;
    }
}
/**
  添加子结点
  */
void BooleanParser::addChild(Node *parent, Node *child)
{
    if (child) {
        parent->children += child;
        parent->str += child->str;
        child->parent = parent;
    }
}
/**
  添加token
  */
void BooleanParser::addToken(Node *parent, const QString &str,
                             Node::Type type)
{
    if (in.mid(pos, str.length()) == str) {
        addChild(parent, new Node(type, str));
        pos += str.length();
    }
}
/**
  token匹配
  */
bool BooleanParser::matchToken(const QString &str) const
{
    return in.mid(pos, str.length()) == str;
}
```

**booleanwindow.h**

``` cpp
#ifndef BOOLEANWINDOW_H
#define BOOLEANWINDOW_H
#include <QWidget>
class QLabel;
class QLineEdit;
class QTreeView;
class BooleanModel;
class BooleanWindow : public QWidget
{
    Q_OBJECT
public:
    BooleanWindow();
private slots:private slots:
    void booleanExpressionChanged(const QString &expr);
private:
    QLabel *label;
    QLineEdit *lineEdit;
    BooleanModel *booleanModel;
    QTreeView *treeView;
};
#endif // BOOLEANWINDOW_H
```

**booleanwindow.cpp**

``` cpp
#include <QtGui>
#include "booleanwindow.h"
#include "booleanmodel.h"
#include "booleanparser.h"
BooleanWindow::BooleanWindow()
{
    //Label
    label = new QLabel(tr("Boolean expression:"));
    //LineEdit
    lineEdit = new QLineEdit;
    //BooleanModel
    booleanModel = new BooleanModel(this);
    //TreeView
    treeView = new QTreeView;
    treeView->setModel(booleanModel);
    //signal and slot
    connect(lineEdit, SIGNAL(textChanged(QString)), this,SLOT(booleanExpressionChanged(QString)));
    //Layout
    QGridLayout *layout = new QGridLayout;
    layout->addWidget(label, 0 ,0);
    layout->addWidget(lineEdit, 0 , 1);
    layout->addWidget(treeView, 1, 0, 1, 2);
    setLayout(layout);
    setWindowTitle(tr("Boolean Parser"));
}
void BooleanWindow::booleanExpressionChanged(const QString &expr)
{
    BooleanParser parser;
    Node *rootNode = parser.parse(expr);
    booleanModel->setRootNode(rootNode);
}
```

其他的可以去下载

下载地址：[http://download.csdn.net/source/1529790">http://download.csdn.net/source/1529790](http://download.csdn.net/source/1529790">http://download.csdn.net/source/1529790)

软件截图：

![image](/images/uploads/2009/07/204053983.p.png?d=20090729205238592)
---

PS:最近比较郁闷.....