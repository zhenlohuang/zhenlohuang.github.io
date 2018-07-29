---
layout: post
title: 'Ubuntu 8.04配置J2ME问题解决'
date: 2008-11-13
categories: [Linux]
tags: [Ubuntu]
keywords: "Ubuntu"
description: 
comments: true
---

装完Eclipse +WTK2.5.2后运行j2me程序发现编译错误，如下

``` java
Exception in thread “AWT-EventQueue-0″ java.lang.NullPointerException
    at com.sun.java.swing.plaf.gtk.GTKLookAndFeel.initSystemColorDefaults(GTKLookAndFeel.java:1267)
    at com.sun.java.swing.plaf.gtk.GTKLookAndFeel.loadStyles(GTKLookAndFeel.java:1509)
    at com.sun.java.swing.plaf.gtk.GTKLookAndFeel.access$000(GTKLookAndFeel.java:37)
    at com.sun.java.swing.plaf.gtk.GTKLookAndFeel$WeakPCL$1.run(GTKLookAndFeel.java:1449)
    at java.awt.event.InvocationEvent.dispatch(InvocationEvent.java:209)
    at java.awt.EventQueue.dispatchEvent(EventQueue.java:597)
    at java.awt.EventDispatchThread.pumpOneEventForFilters(EventDispatchThread.java:273)
    at java.awt.EventDispatchThread.pumpEventsForFilter(EventDispatchThread.java:183)
    at java.awt.EventDispatchThread.pumpEventsForHierarchy(EventDispatchThread.java:173)
    at java.awt.EventDispatchThread.pumpEvents(EventDispatchThread.java:168)
    at java.awt.EventDispatchThread.pumpEvents(EventDispatchThread.java:160)
    at java.awt.EventDispatchThread.run(EventDispatchThread.java:121)
```
如果通过命令行直接启动WTK会遇到如下情况：

```
(<unknown>:14996): Gtk-WARNING **: Attempting to add a widget with type GtkButton to a GtkComboBoxEntry (need an instance of GtkEntry or of a subclass)

(<unknown>:14996): Gtk-CRITICAL **: gtk_widget_realize: assertion `GTK_WIDGET_ANCHORED (widget) || GTK_IS_INVISIBLE (widget)’ failed

(<unknown>:14996): Gtk-CRITICAL **: gtk_paint_box: assertion `style->depth == gdk_drawable_get_depth (window)’ failed

(<unknown>:14996): Gtk-CRITICAL **: gtk_paint_box: assertion `style->depth == gdk_drawable_get_depth (window)’ failed
```
解决办法：
    首先找到你的WTK的安装目录，在bin中找到ktoolbar和emulator这两个文件，这两个是启动WTK和Emulator的两个启动文件，使用vi或者gedit来对这两个文件进行编辑，在两个文件中的相关位置添加如下一行即可：
   －Dswing.systemlaf=”javax.swing.plaf.metal.MetalLookAndFeel” /
    添加后的ktoolbar如下(注意红色标注的那行即可)：

``` bash
#!/bin/sh

javapathtowtk=/usr/java/jdk1.6.0_05/bin/

PRG=$0

# Resolve soft links
while [ -h "$PRG" ]; do
    ls=`/bin/ls -ld “$PRG”`
    link=`/usr/bin/expr “$ls” : ‘.*-> (.*)$’`
    if /usr/bin/expr “$link” : ‘^/’ > /dev/null 2>&1; then
        PRG=”$link”
    else
        PRG=”`/usr/bin/dirname $PRG`/$link”
    fi
done

KVEM_BIN=`dirname $PRG`
KVEM_HOME=`cd $…{KVEM_BIN}/.. ; pwd`
KVEM_LIB=”${KVEM_HOME}/wtklib”
KVEM_API=”${KVEM_HOME}/lib”
export MMAPI_GM_SOUNDBANK=”${KVEM_API}/soundbank.dls”

“${javapathtowtk}java” -Dkvem.home=”${KVEM_HOME}” /
    -Djava.library.path=”${KVEM_HOME}/bin” /
    -Dswing.systemlaf=”javax.swing.plaf.metal.MetalLookAndFeel” /
    -cp “${KVEM_LIB}/kenv.zip:${KVEM_LIB}/ktools.zip:${KVEM_BIN}/JadTool.jar:${KVEM_BIN}/MEKeyTool.jar:${KVEM_LIB}/customjmf.jar:${KVEM_API}/j2me-ws.jar:${KVEM_BIN}/schema2beansdev.jar:${KVEM_BIN}/j2me_sg_ri.jar:${KVEM_BIN}/jaxrpc-impl.jar:${KVEM_BIN}/jaxrpc-api.jar:${KVEM_BIN}/jaxrpc-spi.jar:${KVEM_BIN}/activation.jar:${KVEM_BIN}/mail.jar:${KVEM_BIN}/saaj-api.jar:${KVEM_BIN}/saaj-impl.jar:${KVEM_BIN}/xsdlib.jar:${KVEM_LIB}/nist-sip-1.2.jar:${KVEM_LIB}/JainSipApi1.1.jar:${KVEM_LIB}/jain-sip-presence-proxy.jar” 
    com.sun.kvem.toolbar.Main “$@”
```
修改后的emulator文件内容如下：

``` bash
#!/bin/sh

javapathtowtk=/usr/java/jdk1.6.0_05/bin/

PRG=$0

# Resolve soft links
while [ -h "$PRG" ]; do
    ls=`/bin/ls -ld “$PRG”`
    link=`/usr/bin/expr “$ls” : ‘.*-> (.*)$’`
    if /usr/bin/expr “$link” : ‘^/’ > /dev/null 2>&1; then
        PRG=”$link”
    else
        PRG=”`/usr/bin/dirname $PRG`/$link”
    fi
done

KVEM_BIN=`dirname “$PRG”`
KVEM_HOME=`cd “${KVEM_BIN}/..” ; pwd`
KVEM_LIB=”${KVEM_HOME}/wtklib”
export MMAPI_GM_SOUNDBANK=”${KVEM_HOME}/lib/soundbank.dls”

“${javapathtowtk}java” -Dkvem.home=”${KVEM_HOME}” /
    -Djava.library.path=”${KVEM_HOME}/bin” /
    -Dswing.systemlaf=”javax.swing.plaf.metal.MetalLookAndFeel” /
    -cp “${KVEM_LIB}/kenv.zip:${KVEM_LIB}/ktools.zip:${KVEM_LIB}/customjmf.jar” 
    com.sun.kvem.environment.EmulatorWrapper “$@” 0
```