---
layout: post
title: "Java ClassLoader分析"
date: 2017-06-11 22:59:36 +0800
categories: [Programming, Java]
keywords: java
description: 
comments: true
---

# 什么是ClassLoader
一个Java程序是由若干个class文件组成。当程序在运行时，即会调用该程序的一个入口函数来调用系统的相关功能，而这些功能都被封装在不同的class文件当中，所以经常要从这个class文件中要调用另外一个class文件中的方法，如果另外一个class文件不存在的，则会引发ClassNotFoundException。

程序在启动的时候，并不会一次性加载程序所要用的所有class文件，而是根据程序的需要，通过Java的类加载机制（ClassLoader）来动态加载某个class文件到内存当中的，从而只有class文件被载入到了内存之后，才能被其它class所引用。所以ClassLoader就是用来动态加载class文件到内存当中用的。

# Java ClassLoader的体系结构
现在先看下ClassLoader的体系结构：
![classload-architecture.png](/images/uploads/2017/06/java-classload-architecture.png)

## Bootstrap ClassLoader
BootStrap ClassLoader：称为启动类加载器，是Java类加载层次中最顶层的类加载器，负责加载JDK中的核心类库，如：rt.jar、resources.jar、charsets.jar等。

可以通过如下程序获得该classloader所加载的jar

``` java
System.out.println(System.getProperty("sun.boot.class.path"));
```
程序输出：
```
/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/resources.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/rt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/sunrsasign.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/jsse.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/jce.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/charsets.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/jfr.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/classes
```

如果是scala会在java的基础上额外加载几个jar。
``` scala
scala> println(System.getProperty("sun.boot.class.path"))
/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/resources.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/rt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/sunrsasign.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/jsse.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/jce.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/charsets.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/jfr.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/classes:/Users/zhenlong/DevTools/scala/lib/akka-actor_2.11-2.3.10.jar:/Users/zhenlong/DevTools/scala/lib/config-1.2.1.jar:/Users/zhenlong/DevTools/scala/lib/jline-2.12.1.jar:/Users/zhenlong/DevTools/scala/lib/scala-actors-2.11.0.jar:/Users/zhenlong/DevTools/scala/lib/scala-actors-migration_2.11-1.1.0.jar:/Users/zhenlong/DevTools/scala/lib/scala-compiler.jar:/Users/zhenlong/DevTools/scala/lib/scala-continuations-library_2.11-1.0.2.jar:/Users/zhenlong/DevTools/scala/lib/scala-continuations-plugin_2.11.7-1.0.2.jar:/Users/zhenlong/DevTools/scala/lib/scala-library.jar:/Users/zhenlong/DevTools/scala/lib/scala-parser-combinators_2.11-1.0.4.jar:/Users/zhenlong/DevTools/scala/lib/scala-reflect.jar:/Users/zhenlong/DevTools/scala/lib/scala-swing_2.11-1.0.2.jar:/Users/zhenlong/DevTools/scala/lib/scala-xml_2.11-1.0.4.jar:/Users/zhenlong/DevTools/scala/lib/scalap-2.11.7.jar
```

## Extension ClassLoader
Extension ClassLoader：称为扩展类加载器，负责加载Java的扩展类库，默认加载JAVA_HOME/jre/lib/ext/目下的所有jar。该类在sun.misc.launcher包中。

## App ClassLoader
App ClassLoader：称为应用类加载器，也称为System ClassLoaer，负责加载应用程序CLASSPATH下的所有jar和class文件。该类也在sun.misc.launcher包中。

## User-Defined ClassLoader 
用户还可以根据需要定义自已的ClassLoader，而这些自定义的ClassLoader都必须继承自java.lang.ClassLoader类.

所有的ClassLoader都必须继承自java.lang.ClassLoader，而Bootstrap ClassLoader却不是。它不是一个普通的Java类，底层由C++编写，已嵌入到了JVM内核当中，当JVM启动后，Bootstrap ClassLoader也随着启动，负责加载完核心类库后，并构造Extension ClassLoader和App ClassLoader类加载器。

# ClassLoader加载原理
ClassLoader使用的是**双亲委托模型**来搜索类的，每个ClassLoader实例都**包含(而不是继承)**一个父类加载器的引用。虚拟机内置的类加载器（Bootstrap ClassLoader）本身没有父类加载器，但可以用作其它ClassLoader实例的的父类加载器。

采用双亲委托机制加载类的时候采用如下的几个步骤：

1. 当前ClassLoader首先从自己已经加载的类中查询是否此类已经加载，如果已经加载则直接返回原来已经加载的类；
2. 当前classLoader的缓存中没有找到被加载的类的时候，委托父类加载器去加载，父类加载器采用同样的策略，首先查看自己的缓存，然后委托父类的父类去加载，一直到bootstrp ClassLoader.
3. 当所有的父类加载器都没有加载的时候，再由当前的类加载器加载，并将其放入它自己的缓存中，以便下次有加载请求的时候直接返回。
4. 如果它们都没有加载到这个类时，则抛出ClassNotFoundException异常。否则（找到），将为这个类生成一个类的定义，并将它加载到内存当中，最后返回这个类在内存中的Class实例对象；

总之，class的加载顺序为：cache -> parent classloader -> self.

使用双亲委托模型的好处：

1. 避免重复加载。当父亲已经加载了该类的时候，就没有必要子ClassLoader再加载一次。
2. 提高安全性。如果不使用这种委托模式，那我们就可以随时使用自定义的String来动态替代java核心api中定义的类型，这样会存在非常大的安全隐患，而双亲委托的方式，就可以避免这种情况，因为String已经在启动时就被引导类加载器（Bootstrcp ClassLoader）加载，所以用户自定义的ClassLoader永远也无法加载一个自己写的String，除非你改变JDK中ClassLoader搜索类的默认算法。

在JVM在搜索类的时候，又是如何判定两个class是相同的呢？**JVM在判定两个class是否相同时，不仅要判断两个类名是否相同，而且要判断是否由同一个类加载器实例加载的。只有两者同时满足的情况下，JVM才认为这两个class是相同的。**就算两个class是同一份class字节码，如果被两个不同的ClassLoader实例所加载，JVM也会认为它们是两个不同class。比如有一个Java类SimpleClass.java，javac编译之后生成字节码文件SimpleClass.class，ClassLoaderA和ClassLoaderB这两个类加载器并读取了SimpleClass.class文件，并分别定义出了java.lang.Class实例来表示这个类，对于JVM来说，它们是两个不同的实例对象，但它们确实是同一份字节码文件，如果试图将这个Class实例生成具体的对象进行转换时，就会抛运行时异常java.lang.ClassCaseException，提示这是两个不同的类型。

# 如何自定义ClassLoader

## 何时需要自己定制ClassLoader？
1. 需要加载外部的class而这些class，默认的类加载器是加载不到的。例如，文件系统比较特殊或者需要从网络中加载一个class字节流等。
2. 需要实现class的隔离性。常用的web服务器，如weblogic、tomcat、jetty等都实现这样的类加载器。这些类加载器主要做到：
    1）实现加载Web应用指定目录下的jar和class。
    2）实现部署在容器中的Web应用程共同使用的类库的共享。
    3）实现部署在容器中各个Web应用程序自己私有类库的相互隔离。
    
## 如何自定义ClassLoader
1. 继承java.lang.ClassLoader
2. 覆盖findClass()方法

下面是一个自定义ClassLoader的例子：
``` java
public class MyClassLoader extends ClassLoader {
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        Class clazz = this.findLoadedClass(name);  // optional
        if (clazz == null) {
            byte[] classData = loadClassData(name);
            if (classData == null) {
                throw new ClassNotFoundException();
            }
            clazz = defineClass(name, classData, 0, classData.length);
        }
        return clazz;
    }

    private byte[] loadClassData(String name) {
        // TODO: load class data.
        return null;
    }
}
```



