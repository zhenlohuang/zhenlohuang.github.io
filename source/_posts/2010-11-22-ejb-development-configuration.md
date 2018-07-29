---
layout: post
title: 'EJB开发配置'
date: 2010-11-22
categories: [Programming, Java]
tags: [EJB]
keywords: "EJB"
description: 
comments: true
---

要是感觉配置麻烦的娃，就直接装装个MyEclipse好了，这个就不多说了。

下面的安装步骤所涉及的软件版本都是比较低的，可以换用新版本的，反正不影响效果。

 

一、JDK的安装

1、下载Java开发工具包(JDK1.4及以上：Java Development Kit）。

2、运行J2SDK安装文件jdk-1_5_0_14-windows-i586-p.exe。

3、JDK环境变量的设置：JAVA_HOME, C:/jdk1.5.0_14。

4、CLASSPATH设置： JDK1.5.0安装目录/lib/tools.jar。

5、PATH设置： JDK1.5.0安装目录/bin。

测试：在dos命令窗口下面输入javac看能否找到命令 

 

二、Eclipse的安装 

 1、下载 Eclipse (Eclipse3.1.1版本）

 2、将压缩包文件eclipse-SDK-3.1.1-win32.zip解压到指定的位置。

 3、查看安装完成Eclipse文件夹中的目录结构。

 4、找到安装目录下的eclipse.exe文件双击运行。

 

三、Eclipse多国语言包的安装 

 1、下载 Eclipse3.1.1版本的多国语言包。

 2、将压缩包文件NLpack1-eclipse-SDK-3.1.1a-win32.zip解压后获得的文件存放在Eclipse安装目录下的Language子目录中。

 3、在Eclipse安装目录下创建子目录links, 并在该目录下新建一个文本文件，名称为language.start,在文本文件中键入path=d://eclipse//language。

 4、删除Eclipse安装目录中的configuration子目录下面的org.eclipse.update目录。（要是没有完全汉化，再次执行第四步）

 

四、Eclipse中文本编辑器编码的设置 

1、选择“窗口”菜单中的“首选项”命令。

2、选择“常规”→“编辑器”。

3、设置文本文件编码为UTF-8。

4、在Web and XML里面的JSP Files设置编码为UTF-8。

 

五、安装应用服务器Tomcat

 1、下载 Tomcat。

 2、运行Apache Tomcat 安装文件jakarta-tomcat-5.0.19.exe。

 3、打开IE浏览器，在地址栏输入http://127.0.0.1:8080或者http://localhost:8080 查看Tomcat欢迎页面。

 

六、安装 Eclipse中的Tomcat插件

 1、下载 Tomcat插件。

 2、将压缩包文件tomcatPluginV31beta.zip解开后得到的文件名称为:com.sysdeo.eclipse.tomcat_3.1.0.beta的文件夹放到Eclipse安装目录下的plugins目录下。

 3、重新启动Eclipse。

 4、设置：选择“窗口”菜单中的“首选项”命令。选择Tomcat的相关设置。

 5、设置Tomcat Version 和 Tomcat Home路径

 6、在JVM Setting中设置JRE为jre.1.5.0_14

 7、修改Java编译器配置，首选项->Java->编译器,在内联finally块打钩

 

七、安装 Eclipse中的Lomboz插件

 1、下载 Lomboz插件：

     lomboz-3.1RC2.zip和lomboz-emf-gef-jem-3.1RC.zip

     org.objectweb.lomboz_3.0.1.N20050106

 2、在Eclipse安装目录下分别创建两个文件夹，名称分别为：lomboz和emf。

 3、将lomboz-3.1RC2.zip解压到lomboz文件夹下。

 4、将lomboz-emf-gef-jem-3.1RC.zip解压到emf文件夹下。

 5、在link文件夹下创建两个文件用于指明对应路径。

 6、将org.objectweb.lomboz_3.0.1.N20050106解压到lomboz目录下面

 

八、MySQL的安装

 1、下载 MySQL安装程序：MySQL 4.1.14

 2、解压缩下载的压缩文件，获得名称为setup.exe的安装程序，双击安装程序，开始MySQL数据库系统的安装。

 3、安装MySQL服务器端管理工具

      mysql-administrator-1.1.3-win.msi

 4、安装MySQL客户端查询浏览工具

      mysql-query-browser-1.1.15-win.msi

