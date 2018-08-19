---
title: Spark Troubleshooting - Task not serializable问题分析
tags: Spark
categories:
  - Big Data
  - Spark
date: 2018-08-19 15:34:38
---


# 问题描述
出现“org.apache.spark.SparkException: Task not serializable”这个错误，一般是因为在map、filter等的参数使用了外部的变量，但是这个变量不能序列化。其中最普遍的情形是：当引用了某个类（经常是当前类）的成员函数或变量时，会导致这个类的所有成员（整个类）都需要支持序列化。虽然许多情形下，当前类使用了“extends Serializable”声明支持序列化，但是由于某些字段不支持序列化，仍然会导致整个类序列化时出现问题，最终导致出现Task未序列化问题。

# 解决办法与编程建议
这个问题主要是引用了某类的成员变量或函数，并且相应的类没有做好序列化处理导致的。因此解决这个问题无非以下两种方法：

## 不在（或不直接在）map等闭包内部直接引用某类成员函数或成员变量

* 对于依赖某类成员变量的情形
如果程序依赖的值相对固定，可取固定的值，或定义在map、filter等操作内部，或定义在scala object对象中。
如果依赖值需要程序调用时动态指定（以函数参数形式），则在map、filter等操作时，可不直接引用该成员变量，而是根据成员变量的值重新定义一个局部变量，这样map等算子就无需引用类的成员变量。

* 对于依赖某类成员函数的情形
如果函数功能独立，可定义在scala object对象中（类似于Java中的static方法），这样就无需一来特定的类。

## 如果引用了某类的成员函数或变量，则需对相应的类做好序列化处理

首先该类继承Serializable接口，然后对于不能序列化的成员变量使用“@transent”标注，告诉编译器不需要序列化。 此外如果可以，可将依赖的变量独立放到一个小的class中，让这个class支持序列化，这样做可以减少网络传输量，提高效率。


# 参考资料
* https://blog.csdn.net/javastart/article/details/51206715


