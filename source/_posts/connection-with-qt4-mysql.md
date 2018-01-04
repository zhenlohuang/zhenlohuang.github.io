---
layout: post
title: '用Qt4 连接MySQL'
date: 2009-7-29
wordpress_id: 333
categories: [Programming, C++]
tags: [Qt, C++, MySQL]
keywords: "Qt, C++, MySQL"
description: 
comments: true
---
最近学习Qt4的数据库编程方面，早上安装了下MySQL，这里写Qt4连接MySQL的方法

首先，自己先去建立一个数据库名为，DB_Name

然后，自己可以尝试下连接，利用mysql-admin是个很好用的工具，保证数据连接是没有问题的，这里用的Server Name 就用本地的吧(localhost)

保证数据库连接没问题以后，就可以参考下这下面的代码了（仅供参考，有问题请来信），我把它写成函数的形式，方便以后调用

``` cpp
bool createConnection()
{
    QSqlDatabase db = QSqlDatabase::addDatabase("QMYSQL");
    db.setHostName("localhost");
    db.setDatabaseName("DB_Name");
    db.setUserName("root");
    db.setPassword("123456");
    if (!db.open()) {
        QMessageBox::warning(0, QObject::tr("Database Error"),
                             db.lastError().text());
        return false;
    }
    return true;
}
```

Sever Name：localhost使用本地连接

数据库名：DB_Name

登录账户：root

密码：123456
