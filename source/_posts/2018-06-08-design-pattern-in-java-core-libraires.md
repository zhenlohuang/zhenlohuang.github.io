---
title: Java中的设计模式
date: 2018-06-08
categories: [Programming, Java]
---
《设计模式：可复用面向对象软件的基础》一书中提出了24中经典的设计模式，这些设计模式被广泛地运用于项目实战之中。这篇博客的重点并不在于讲解这些设计模式以及使用，而主要列举了在Java Core Libraries中间所用到的设计模式。Java Core Libraries中的API设计基本涵盖了GOF书中所提到的绝大部分设计模式，可以说是API设计的典范，非常值得借鉴和学习。

# 创建型模式
这些设计模式提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是使用 new 运算符直接实例化对象。这使得程序在判断针对某个给定实例需要创建哪些对象时更加灵活。

## 工厂模式（Factory Pattern）
* java.util.Calendar#getInstance()
* java.util.ResourceBundle#getBundle()
* java.text.NumberFormat#getInstance()
* java.nio.charset.Charset#forName()
* java.net.URLStreamHandlerFactory#createURLStreamHandler(String) (Returns singleton object per protocol)
* java.util.EnumSet#of()
* javax.xml.bind.JAXBContext#createMarshaller() and other similar methods

## 抽象工厂模式（Abstract Factory Pattern）
* javax.xml.parsers.DocumentBuilderFactory#newInstance()
* javax.xml.transform.TransformerFactory#newInstance()
* javax.xml.xpath.XPathFactory#newInstance()

## 单例模式（Singleton Pattern）
* java.lang.Runtime#getRuntime()
* java.awt.Desktop#getDesktop()
* java.lang.System#getSecurityManager()

## 建造者模式（Builder Pattern）
* java.lang.StringBuilder#append() (unsynchronized)
* java.lang.StringBuffer#append() (synchronized)
* java.nio.ByteBuffer#put() (also on CharBuffer, ShortBuffer, IntBuffer, LongBuffer, FloatBuffer and DoubleBuffer)
* javax.swing.GroupLayout.Group#addComponent()

## 原型模式（Prototype Pattern）
* java.lang.Object#clone()

# 结构型模式
这些设计模式关注类和对象的组合。继承的概念被用来组合接口和定义组合对象获得新功能的方式

## 适配器模式（Adapter Pattern）
* java.util.Arrays#asList()
* java.util.Collections#list()
* java.util.Collections#enumeration()
* java.io.InputStreamReader(InputStream) (returns a Reader)
* java.io.OutputStreamWriter(OutputStream) (returns a Writer)
* javax.xml.bind.annotation.adapters.XmlAdapter#marshal() and #unmarshal()

## 桥接模式（Bridge Pattern）
TODO

## 过滤器模式（Filter、Criteria Pattern）

## 组合模式（Composite Pattern）
* java.awt.Container#add(Component)
* javax.faces.component.UIComponent#getChildren()

## 装饰器模式（Decorator Pattern）
* All subclasses of java.io.InputStream, OutputStream, Reader and Writer have a constructor taking an instance of same type.
* java.util.Collections, the checkedXXX(), synchronizedXXX() and unmodifiableXXX() methods.
* javax.servlet.http.HttpServletRequestWrapper and HttpServletResponseWrapper

## 外观模式（Facade Pattern）
* javax.faces.context.FacesContext, it internally uses among others the abstract/interface types LifeCycle, ViewHandler, NavigationHandler and many more without that the enduser has to worry about it (which are however overrideable by injection).
* javax.faces.context.ExternalContext, which internally uses ServletContext, HttpSession, HttpServletRequest, HttpServletResponse, etc.

## 享元模式（Flyweight Pattern）
* java.lang.Integer#valueOf(int) (also on Boolean, Byte, Character, Short, Longand BigDecimal)

## 代理模式（Proxy Pattern）
* java.lang.reflect.Proxy
* java.rmi.*
* javax.ejb.EJB (explanation here)
* javax.inject.Inject (explanation here)
* javax.persistence.PersistenceContext

# 行为型模式
## 责任链模式（Chain of Responsibility Pattern）
* java.util.logging.Logger#log()
* javax.servlet.Filter#doFilter()

## 命令模式（Command Pattern）
* All implementations of java.lang.Runnable
* All implementations of javax.swing.Action

## 解释器模式（Interpreter Pattern）
* java.util.Pattern
* java.text.Normalizer
* All subclasses of java.text.Format
* All subclasses of javax.el.ELResolver

## 迭代器模式（Iterator Pattern）
* All implementations of java.util.Iterator (thus among others also java.util.Scanner!).
* All implementations of java.util.Enumeration

## 中介者模式（Mediator Pattern）
* java.util.Timer (all scheduleXXX() methods)
* java.util.concurrent.Executor#execute()
* java.util.concurrent.ExecutorService (the invokeXXX() and submit() methods)
* java.util.concurrent.ScheduledExecutorService (all scheduleXXX() methods)
* java.lang.reflect.Method#invoke()

## 备忘录模式（Memento Pattern）
* java.util.Date (the setter methods do that, Date is internally represented by a longvalue)
* All implementations of java.io.Serializable
* All implementations of javax.faces.component.StateHolder

## 观察者模式（Observer Pattern）
* java.util.Observer/java.util.Observable (rarely used in real world though)*
* All implementations of java.util.EventListener (practically all over Swing thus)
* javax.servlet.http.HttpSessionBindingListener
* javax.servlet.http.HttpSessionAttributeListener
* javax.faces.event.PhaseListener

## 状态模式（State Pattern）
* javax.faces.lifecycle.LifeCycle#execute() (controlled by FacesServlet, the behaviour is dependent on current phase (state) of JSF lifecycle)

## 空对象模式（Null Object Pattern）
TODO

## 策略模式（Strategy Pattern）
* java.util.Comparator#compare(), executed by among others Collections#sort().
* javax.servlet.http.HttpServlet, the service() and all doXXX() methods take HttpServletRequest and HttpServletResponse and the implementor has to process them (and not to get hold of them as instance variables!).
* javax.servlet.Filter#doFilter()

## 模板模式（Template Pattern）
* All non-abstract methods of java.io.InputStream, java.io.OutputStream, java.io.Reader and java.io.Writer.
* All non-abstract methods of java.util.AbstractList, java.util.AbstractSet and java.util.AbstractMap.
javax.servlet.http.HttpServlet, all the doXXX() methods by default sends a HTTP 405 "Method Not Allowed" error to the response. You're free to implement none or any of them.

## 访问者模式（Visitor Pattern）
* javax.lang.model.element.AnnotationValue and AnnotationValueVisitor
* javax.lang.model.element.Element and ElementVisitor
* javax.lang.model.type.TypeMirror and TypeVisitor
* java.nio.file.FileVisitor and SimpleFileVisitor
* javax.faces.component.visit.VisitContext and VisitCallback

# 参考资料
* http://www.runoob.com/design-pattern/design-pattern-intro.html
* https://stackoverflow.com/questions/1673841/examples-of-gof-design-patterns-in-javas-core-libraries

