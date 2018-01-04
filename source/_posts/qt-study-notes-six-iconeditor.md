---
layout: post
title: 'QT学习笔记之六 IconEditor'
date: 2009-5-1
wordpress_id: 286
categories: [Programming, C++]
tags: [Qt, C++]
keywords: "Qt, C++"
description: 
comments: true
---
这次写了一个简单的Icon编辑器，功能很简单的说.....详见代码

=================================================================

iconeditor.h

``` cpp
#ifndef ICONEDITOR_H
#define ICONEDITOR_H
#include <QtGui/QWidget>
#include <QImage>
#include <QWidget>
class IconEditor : public QWidget
{
    Q_OBJECT
    Q_PROPERTY(QColor penColor READ penClor WRITE setPenColor);
    Q_PROPERTY(QImage iconImage READ iconImage WRITE setIconImage);
    Q_PROPERTY(int zoomFactor READ zoomFactor WRITE setZoomFactor);
public:
    IconEditor(QWidget *parent = 0);
    ~IconEditor();
    void setPenColor(const QColor &newColor);
    QColor penColor();
    void setZoomFactor(int newZoom);
    int zoomFactor() const;
    void setIconImage(const QImage &newImage);
    QImage iconImage() const;
    QSize sizeHint() const;
protected:
    void mousePressEvent(QMouseEvent *);
    void mouseMoveEvent(QMouseEvent *);
    void paintEvent(QPaintEvent *event);
private:
    void setImagePixel(const QPoint &pos,bool opaque);
    QRect pixelRect(int i,int j) const;
    QColor curColor;
    QImage image;
    int zoom;
};
#endif // ICONEDITOR_H
```

iconeditor.cpp

``` cpp
#include<QtGui>
#include "iconeditor.h"
IconEditor::IconEditor(QWidget *parent)
    : QWidget(parent)
{
    setAttribute(Qt::WA_StaticContents);
    setSizePolicy(QSizePolicy::Minimum,QSizePolicy::Minimum);
    curColor=Qt::black;
    zoom=8;
    image=QImage(16,16,QImage::Format_ARGB32);
    image.fill(qRgba(0,0,0,0));
}
/*
Qt::WA_StaticContents 用法：
Indicates that the widget contents are north-west aligned and static.
On resize, such a widget will receive paint events only for parts of itself
that are newly visible. This flag is set or cleared by the widget's author.
*/
IconEditor::~IconEditor()
{
}
void IconEditor::setPenColor(const QColor &newColor)
{
    curColor=newColor;
}
QColor IconEditor::penColor()
{
    return curColor;
}
void IconEditor::setZoomFactor(int newZoom)
{
    if(newZoom<1)
        newZoom=1;
    if(newZoom!=zoom){
        zoom=newZoom;
        this->update();
        this->updateGeometry();    //调用update和updateGeometry进行重绘
    }
}
int IconEditor::zoomFactor() const
{
    return zoom;
}
void IconEditor::setIconImage(const QImage &newImage)
{
    if(newImage!=image){
        image=newImage.convertToFormat(QImage::Format_ARGB32);
        this->update();
        this->updateGeometry();
    }
}
QImage IconEditor::iconImage() const
{
    return image;
}
QSize IconEditor::sizeHint() const
{
    QSize size=zoom*image.size();
    if(zoom>=3)
        size+=QSize(1,1);    //当zoom>=3时，增加一个像素用存放网格线
    return size;
}
/*只要重绘就会调用这个函数*/
void IconEditor::paintEvent(QPaintEvent *event)
{
    QPainter painter(this);
    if(zoom>=3){
        painter.setPen(palette().foreground().color());
        //绘制垂直表格线
        for(int i=0;i<=image.height();++i)
            painter.drawLine(zoom*i,0,zoom*image.width(),zoom*i);   //(x1,y1) to (x2,y2)
        //绘制水平表格线
        for(int j=0;j<image.width();++j)
            painter.drawLine(0,zoom*j,zoom*image.width(),zoom*j);
    }
    for(int i=0;i<image.width();++i){
        for(int j=0;j<image.height();++j){
            QRect rect=pixelRect(i,j);
            if(!event->region().intersect(rect).isEmpty()){   //region区域
                QColor color=QColor::fromRgba(image.pixel(i,j));   //pixel(x,y)返回(x,y)的一个像素
                if(color.alpha()<255)
                    painter.fillRect(rect,Qt::white);
                painter.fillRect(rect,color);
            }
        }
    }
}
QRect IconEditor::pixelRect(int i,int j) const
{
    if(zoom>=3){
        return QRect(zoom*i+1,zoom*j+1,zoom-1,zoom-1);
    }else{
        return QRect(zoom*i,zoom*j,zoom,zoom);  //QRect其中一种构造函数，QRect ( int x, int y, int width, int height )
    }
}
void IconEditor::mousePressEvent(QMouseEvent *event)
{
    if(event->button()==Qt::LeftButton){
        setImagePixel(event->pos(),true);
    }else if(event->button()==Qt::RightButton){
        setImagePixel(event->pos(),false);
    }
}
void IconEditor::mouseMoveEvent(QMouseEvent *event)
{
    if(event->buttons()& Qt::LeftButton){    //用Qt::LeftButton与buttons位或
        setImagePixel(event->pos(),true);
    }else if(event->buttons()& Qt::RightButton){
        setImagePixel(event->pos(),false);
    }
}
void IconEditor::setImagePixel(const QPoint &pos,bool opaque)
{
    int i=pos.x()/zoom;
    int j=pos.y()/zoom;
    if(image.rect().contains(i,j)){
        if(opaque){
            image.setPixel(i,j,penColor().rgba());
        }else{
            image.setPixel(i,j,qRgba(0,0,0,0));
        }
    }
        update(pixelRect(i,j));
}
```

main.cpp

``` cpp
#include <QtGui/QApplication>
#include "iconeditor.h"
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    IconEditor *iconEditor=new IconEditor;
    iconEditor->setWindowTitle("Icon Editor");
    iconEditor->setIconImage(QImage(":/images/qt-logo.png"));
    iconEditor->show();
    return app.exec();
}
```

---

PS:貌似无话可说
