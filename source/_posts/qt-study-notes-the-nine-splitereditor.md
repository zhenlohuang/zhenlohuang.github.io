---
layout: post
title: 'QT学习笔记之九 SpliterEditor'
date: 2009-7-3
wordpress_id: 312
categories: [Programming]
tags: [Qt]
keywords: "Qt"
description: 
comments: true
---
没话说...直接贴代码

**splitereditor.h**

``` cpp
#ifndef SPLITEREDITOR_H
#define SPLITEREDITOR_H
#include <QtGui/QWidget>
class QSplitter;
class QTextEdit;
namespace Ui
{
    class SpliterEditor;
}
class SpliterEditor : public QWidget
{
    Q_OBJECT
public:
    SpliterEditor(QWidget *parent = 0);
    ~SpliterEditor();
private:
    Ui::SpliterEditor *ui;
    QSplitter *splitter;
    QTextEdit *editor1;
    QTextEdit *editor2;
    QTextEdit *editor3;
};
#endif // SPLITEREDITOR_H
```

**splitereditor.cpp**

```
#include <QtGui>
#include "splitereditor.h"
#include "ui_splitereditor.h"
SpliterEditor::SpliterEditor(QWidget *parent)
    : QWidget(parent), ui(new Ui::SpliterEditor)
{
    ui->setupUi(this);
    splitter = new QSplitter(Qt::Horizontal);
    editor1 = new QTextEdit;
    editor2 = new QTextEdit;
    editor3 = new QTextEdit;
    splitter->addWidget(editor1);
    splitter->addWidget(editor2);
    splitter->addWidget(editor3);
    editor1->setPlainText("Killua");
    editor2->setPlainText("KK");
    editor3->setPlainText("Hello World");
    splitter->show();
}
SpliterEditor::~SpliterEditor()
{
    delete ui;
}
```
