---
layout: post
title: 'QT学习笔记之八 MDIEditor'
date: 2009-7-2
wordpress_id: 311
permalink: /archives/qt-study-notes-eight-mdieditor.html
categories: [Programming]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
这个程序由于期末考试搁置很久了，下午由于下大雨没出去，就把他完成了哈.....主要实现多文档编辑功能，测试了下，基本上没有bug，由于整体设计思路参考书上的，大牛们不要BS我阿.....

等下次有时间将他改成简易的代码编辑器好了，貌似国外网站上有相关模块，有时间去看看。。。。

---

程序部分代码如下：

**editor.h**

``` cpp
#ifndef EDITOR_H
#define EDITOR_H
#include <QTextEdit>
class Editor : public QTextEdit
{
    Q_OBJECT
public:
    Editor(QWidget *parent=0);
    void newFile();
    bool save();
    bool saveAs();
    QSize sizeHint() const;
    QAction *windowMenuAction() const { return action ;}
    static Editor *open(QWidget *parent = 0);
    static Editor *openFile(const QString &fileName,QWidget *parent = 0);
protected:
    void closeEvent(QCloseEvent *event);
private slots:
    void documentWasModified();
private:
    bool okToContinue();
    bool saveFile(const QString &fileName);
    void setCurrentFile(const QString &fileName);
    bool readFile(const QString &fileName);
    bool writeFile(const QString &fileName);
    QString strippedName(const QString &fullFileName);
    QString curFile;
    bool isUntitled;
    QAction *action;
};
#endif // EDITOR_H
```

**editor.cpp**

``` cpp
#include <QtGui>
#include "editor.h"
/**
  构造函数
*/
Editor::Editor(QWidget *parent) :QTextEdit(parent)
{
    action = new QAction(this);
    action->setCheckable(true);
    connect(action,SIGNAL(triggered()),this,SLOT(show()));
    connect(action,SIGNAL(triggered()),this,SLOT(setFocus()));
    isUntitled = true;
    connect(document(),SIGNAL(contentsChanged()),this,SLOT(documentWasModified()));
    setWindowIcon(QPixmap(":/images/document.png"));
    setWindowTitle("[*]");
    setAttribute(Qt::WA_DeleteOnClose);  //Makes Qt delete this widget when the widget has accepted the close event
}
/**
  新建文件
*/
void Editor::newFile()
{
    static int documentNumber = 1;
    curFile = tr("document%1.txt").arg(documentNumber);
    setWindowTitle(curFile + "[*]");
    action->setText(curFile);
    isUntitled= true;
    ++documentNumber;
}
/**
  保存
*/
bool Editor::save()
{
    if(isUntitled) {
        return saveAs();
    }
    else {
        return saveFile(curFile);
    }
}
/**
  另存为
*/
bool Editor::saveAs()
{
    QString fileName = QFileDialog::getSaveFileName(this,tr("Save As"),curFile);
    if(fileName.isEmpty())
        return false;
    return saveFile(fileName);
}
/**
  大小调整
*/
QSize Editor::sizeHint() const
{
    return QSize(72 * fontMetrics().width('x'),25* fontMetrics().lineSpacing());  //Returns the font metrics for the widget's current font
}
/**
  打开
*/
Editor *Editor::open(QWidget *parent)
{
    QString fileName = QFileDialog::getOpenFileName(parent,tr("Open"),".");
    if(fileName.isEmpty())
        return 0;
    return openFile(fileName,parent);
}
/**
  打开文件
*/
Editor *Editor::openFile(const QString &fileName,QWidget *parent)
{
    Editor *editor = new Editor(parent);
    if(editor->readFile(fileName)) {
        editor->setCurrentFile(fileName);
        return editor;
    }
    else {
        delete editor;
        return 0;
    }
}
/**
  关闭事件
*/
void Editor::closeEvent(QCloseEvent *event)
{
    if(okToContinue()) {
        event->accept();
    }
    else {
        event->ignore();
    }
}
/**
  文件已被修改
*/
void Editor::documentWasModified()
{
    setWindowModified(true);
}
/**
  是否可以继续
*/
bool Editor::okToContinue()
{
    if(document()->isModified()) {
        int r=QMessageBox::warning(this,tr("MDI Editor"),
                                   tr("File %1 has been modified./n").arg(strippedName(curFile),
                                   QMessageBox::Yes | QMessageBox::No | QMessageBox::Cancel));
        if(r == QMessageBox::Yes) {
            save();
        }
        else if (r == QMessageBox::Cancel) {
            return false;
        }
    }
    return true;
}
/**
  文件保存
*/
bool Editor::saveFile(const QString &fileName)
{
    if (writeFile(fileName)) {
        setCurrentFile(fileName);
        return true;
    }
    else {
        return false;
    }
}
/**
  设置为当前文件
*/
void Editor::setCurrentFile(const QString &fileName)
{
    curFile = fileName;
    isUntitled = false;
    action->setText(strippedName(curFile));
    document()->setModified(false);
    setWindowTitle(strippedName(curFile)+ "[*]");
    setWindowModified(false);
}
/**
  读取文件
*/
bool Editor::readFile(const QString &fileName)
{
    QFile file(fileName);
    if(!file.open(QIODevice::ReadOnly | QIODevice::Text)) {   //设置文件读取模式
        QMessageBox::warning(this,tr("MDI Editor"),tr("Cannot read file %1:/n%2.").
                             arg(file.fileName()).arg(file.errorString()));
        return false;
    }
    QTextStream in(&file);
    QApplication::setOverrideCursor(Qt::WaitCursor);  //设置鼠标
    setPlainText(in.readAll());
    QApplication::restoreOverrideCursor();
    return true;
}
/**
  文件写入
*/
bool Editor::writeFile(const QString &fileName)
{
    QFile file(fileName);
    if(!file.open(QIODevice::WriteOnly | QIODevice::Text)) {
        QMessageBox::warning(this,tr("MDI Editor"),
                             tr("Cannot write file %1:/n%2").arg(file.fileName()).arg(file.errorString()));
        return false;
    }
    QTextStream out(&file);
    QApplication::setOverrideCursor(Qt::WaitCursor);
    out<<toPlainText()
            ;
    QApplication::restoreOverrideCursor();
    return true;
}
QString Editor::strippedName(const QString &fullFileName)
{
    return QFileInfo(fullFileName).fileName();
}
```

*mainwindow.h*

``` cpp
#ifndef MAINWINDOW_H
#define MAINWINDOW_H
#include <QtGui/QMainWindow>
class QAction;
class QActionGroup;
class QLabel;
class QMdiArea;
class QMenu;
class QToolBar;
class Editor;
namespace Ui
{
    class MainWindow;
}
class MainWindow : public QMainWindow
{
    Q_OBJECT
public:
    MainWindow(QWidget *parent = 0);
    ~MainWindow();
public slots:
    void newFile();
    void openFile(const QString &fileName);
protected:
    void closeEvent(QCloseEvent *event);
private slots:
    void open();
    void save();
    void saveAs();
    void cut();
    void copy();
    void paste();
    void about();
    void updateActions();
    void loadFiles();
private:
    Ui::MainWindow *ui;
    void createActions();
    void createMenus();
    void createToolBars();
    void createStatusBar();
    void addEditor(Editor *editor);
    Editor *activeEditor();
    QMdiArea *mdiArea;    //The QMdiArea widget provides an area in which MDI windows are displayed
    QLabel *readyLabel;
    QWidgetList windows;
    //Menu
    QMenu *fileMenu;
    QMenu *editMenu;
    QMenu *windowMenu;
    QMenu *helpMenu;
    //ToolBar
    QToolBar *fileToolBar;
    QToolBar *editToolBar;
    //Actions
    QActionGroup *windowActionGroup;
    QAction *newAction;
    QAction *openAction;
    QAction *saveAction;
    QAction *saveAsAction;
    QAction *exitAction;
    QAction *cutAction;
    QAction *copyAction;
    QAction *pasteAction;
    QAction *closeAction;
    QAction *closeAllAction;
    QAction *tileAction;
    QAction *cascadeAction;
    QAction *nextAction;
    QAction *previousAction;
    QAction *separatorAction;
    QAction *aboutAction;
    QAction *aboutQtAction;
};
#endif // MAINWINDOW_H
```

**mainwindos.cpp**

``` cpp
#include <QtGui>
#include "mainwindow.h"
#include "ui_mainwindow.h"
#include "editor.h"
MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent), ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    mdiArea = new QMdiArea;
    setCentralWidget(mdiArea);
    connect(mdiArea, SIGNAL(subWindowActivated(QMdiSubWindow*)),this, SLOT(updateActions()));
    createActions();
    createMenus();
    createToolBars();
    createStatusBar();
    setWindowIcon(QPixmap(":/images/icon.png"));
    setWindowTitle("Killua MDI Editor");
    QTimer::singleShot(0,this,SLOT(loadFiles()));
}
MainWindow::~MainWindow()
{
    delete ui;
}
/**
   文件读取
  */
void MainWindow::loadFiles()
{
    QStringList args = QApplication::arguments();
    args.removeFirst();
    if (!args.isEmpty()) {
        foreach(QString arg,args)
            openFile(arg);
        mdiArea->cascadeSubWindows();
    } else {
        newFile();
    }
    mdiArea->activateNextSubWindow();
}
/**
  新建文件
  */
void MainWindow::newFile()
{
    Editor *editor = new Editor;
    editor->newFile();
    addEditor(editor);
}
/**
  打开文件
  */
void MainWindow::openFile(const QString &fileName)
{
    Editor *editor = Editor::openFile(fileName,this);
    if(editor)
        addEditor(editor);
}
/**
  关闭事件
  */
void MainWindow::closeEvent(QCloseEvent *event)
{
    mdiArea->closeAllSubWindows();
    if(!mdiArea->subWindowList().isEmpty()) {
        event->ignore();
    } else {
        event->accept();
    }
}
/**
  打开操作
  */
void MainWindow::open()
{
    Editor *editor = Editor::open(this);
    if(editor)
        addEditor(editor);
}
/**
  保存文件
  */
void MainWindow::save()
{
    if(activeEditor())
        activeEditor()->save();
}
/**
  文件另存为
  */
void MainWindow::saveAs()
{
    if (activeEditor())
        activeEditor()->saveAs();
}
/**
  剪切
  */
void MainWindow::cut()
{
    if(activeEditor())
        activeEditor()->cut();
}
/**
  复制
  */
void MainWindow::copy()
{
    if(activeEditor())
        activeEditor()->copy();
}
/**
  粘贴
  */
void MainWindow::paste()
{
    if(activeEditor())
        activeEditor()->paste();
}
/**
  关于
  */
void MainWindow::about()
{
    QMessageBox::about(this,tr("About Killua MDI Editor"),
                       tr("Design by Killua"));
}
/**
  事件更新
  */
void MainWindow::updateActions()
{
    bool hasEditor = (activeEditor()!=0);
    bool hasSelection = activeEditor() && activeEditor()->textCursor().hasSelection();
    saveAction->setEnabled(hasEditor);
    saveAsAction->setEnabled(hasEditor);
    cutAction->setEnabled(hasSelection);
    copyAction->setEnabled(hasSelection);
    pasteAction->setEnabled(hasEditor);
    closeAction->setEnabled(hasEditor);
    closeAllAction->setEnabled(hasEditor);
    tileAction->setEnabled(hasEditor);
    cascadeAction->setEnabled(hasEditor);
    nextAction->setEnabled(hasEditor);
    previousAction->setEnabled(hasEditor);
    separatorAction->setEnabled(hasEditor);
    if(activeEditor())
        activeEditor()->windowMenuAction()->setChecked(true);  //菜单栏可点击
}
/**
  事件创建
  */
void MainWindow::createActions()
{
    //newAction
    newAction = new QAction(tr("&New"), this);
    newAction->setIcon(QIcon(":/images/new.png"));
    newAction->setShortcut(QKeySequence::New);
    connect(newAction, SIGNAL(triggered()), this, SLOT(newFile()));
    //openAction
    openAction = new QAction(tr("&Open..."), this);
    openAction->setIcon(QIcon(":/images/open.png"));
    openAction->setShortcut(QKeySequence::Open);
    connect(openAction, SIGNAL(triggered()), this, SLOT(open()));
    //saveAction
    saveAction = new QAction(tr("&Save"), this);
    saveAction->setIcon(QIcon(":/images/save.png"));
    saveAction->setShortcut(QKeySequence::Save);
    connect(saveAction, SIGNAL(triggered()), this, SLOT(save()));
    //saveAsAction
    saveAsAction = new QAction(tr("Save &As..."), this);
    connect(saveAsAction, SIGNAL(triggered()), this, SLOT(saveAs()));
    //exitAction
    exitAction = new QAction(tr("E&xit"), this);
    exitAction->setShortcut(tr("Ctrl+Q"));
    connect(exitAction, SIGNAL(triggered()), this, SLOT(close()));
    //cutAction
    cutAction = new QAction(tr("Cu&t"), this);
    cutAction->setIcon(QIcon(":/images/cut.png"));
    cutAction->setShortcut(QKeySequence::Cut);
    connect(cutAction, SIGNAL(triggered()), this, SLOT(cut()));
    //copyAction
    copyAction = new QAction(tr("&Copy"), this);
    copyAction->setIcon(QIcon(":/images/copy.png"));
    copyAction->setShortcut(QKeySequence::Copy);
    connect(copyAction, SIGNAL(triggered()), this, SLOT(copy()));
    //pasteAction
    pasteAction = new QAction(tr("&Paste"), this);
    pasteAction->setIcon(QIcon(":/images/paste.png"));
    pasteAction->setShortcut(QKeySequence::Paste);
    connect(pasteAction, SIGNAL(triggered()), this, SLOT(paste()));
    //closeAction
    closeAction = new QAction(tr("Cl&ose"), this);
    closeAction->setShortcut(QKeySequence::Close);
    connect(closeAction, SIGNAL(triggered()),mdiArea, SLOT(closeActiveSubWindow()));
    //closeAllAction
    closeAllAction = new QAction(tr("Close &All"), this);
    connect(closeAllAction, SIGNAL(triggered()), this, SLOT(close()));
    //tileAction
    tileAction = new QAction(tr("&Tile"), this);
    connect(tileAction, SIGNAL(triggered()),mdiArea, SLOT(tileSubWindows()));
    //cacadeAction
    cascadeAction = new QAction(tr("&Cascade"), this);
    connect(cascadeAction, SIGNAL(triggered()),mdiArea, SLOT(cascadeSubWindows()));
    //newAction
    nextAction = new QAction(tr("Ne&xt"), this);
    nextAction->setShortcut(QKeySequence::NextChild);
    connect(nextAction, SIGNAL(triggered()),mdiArea, SLOT(activateNextSubWindow()));
    //previousAction
    previousAction = new QAction(tr("Pre&vious"), this);
    previousAction->setShortcut(QKeySequence::PreviousChild);
    connect(previousAction, SIGNAL(triggered()),mdiArea, SLOT(activatePreviousSubWindow()));
    //separatorAction
    separatorAction = new QAction(this);
    separatorAction->setSeparator(true);
    //aboutAction
    aboutAction = new QAction(tr("&About"), this);
    aboutAction->setStatusTip(tr("Show the application's About box"));
    connect(aboutAction, SIGNAL(triggered()), this, SLOT(about()));
    //aboutQtAction
    aboutQtAction = new QAction(tr("About &Qt"), this);
    connect(aboutQtAction, SIGNAL(triggered()), qApp, SLOT(aboutQt()));
    windowActionGroup = new QActionGroup(this);
}
/**
  菜单栏创建
  */
void MainWindow::createMenus()
{
    //fileMenu
    fileMenu = menuBar()->addMenu("&File");
    fileMenu->addAction(newAction);
    fileMenu->addAction(openAction);
    fileMenu->addAction(saveAction);
    fileMenu->addAction(saveAsAction);
    fileMenu->addSeparator();
    fileMenu->addAction(exitAction);
    //editMenu
    editMenu = menuBar()->addMenu(tr("&Edit"));
    editMenu->addAction(cutAction);
    editMenu->addAction(copyAction);
    editMenu->addAction(pasteAction);
    windowMenu=menuBar()->addMenu(tr("&Windos"));
    windowMenu->addAction(closeAction);
    windowMenu->addAction(closeAllAction);
    windowMenu->addSeparator();
    windowMenu->addAction(tileAction);
    windowMenu->addAction(cascadeAction);
    windowMenu->addSeparator();
    windowMenu->addAction(nextAction);
    windowMenu->addAction(previousAction);
    windowMenu->addAction(separatorAction);
    menuBar()->addSeparator();
    helpMenu = menuBar()->addMenu(tr("&Help"));
    helpMenu->addAction(aboutAction);
    helpMenu->addAction(aboutQtAction);
}
/**
  创建工具栏
  */
void MainWindow::createToolBars()
{
    fileToolBar = addToolBar(tr("FIle"));
    fileToolBar->addAction(newAction);
    fileToolBar->addAction(openAction);
    fileToolBar->addAction(saveAction);
    editToolBar = addToolBar(tr("Edit"));
    editToolBar->addAction(cutAction);
    editToolBar->addAction(copyAction);
    editToolBar->addAction(pasteAction);
}
/**
  创建状态栏
  */
void MainWindow::createStatusBar()
{
    readyLabel = new QLabel(tr("Ready"));
    statusBar()->addWidget(readyLabel,1);
}
/**
  添加Editor
  */
void MainWindow::addEditor(Editor *editor)
{
    connect(editor,SIGNAL(copyAvailable(bool)),cutAction,SLOT(setEnabled(bool)));
    connect(editor,SIGNAL(copyAvailable(bool)),copyAction,SLOT(setEnabled(bool)));
    QMdiSubWindow *subWindow = mdiArea->addSubWindow(editor);
    windowMenu->addAction(editor->windowMenuAction());
    subWindow->show();
}
/**
  激活窗口
  */
Editor *MainWindow::activeEditor()
{
    QMdiSubWindow *subWindow = mdiArea->activeSubWindow();
    if(subWindow)
        return qobject_cast<Editor *>(subWindow->widget());
    return 0;
}
```

*PS:完整源代码放到资源里面了，附上链接......*

![http://download.csdn.net/source/1458294](http://download.csdn.net/source/1458294)

---
放假了...无聊中....

