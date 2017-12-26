---
layout: post
title: 'hadoop-eclipse-plugin编译及安装'
date: 2012-11-26
wordpress_id: 3475
permalink: /archives/hadoop-eclipse-plugin-compiled.html
categories: [Big Data]
tags: [Hadoop]
keywords: "Hadoop"
description: 
comments: true
---
OS: Ubunut 12.04

Hadoop: 1.0.4

JDK: OpenJDK 1.6

1.修改hadoop/src/contrib/build-contrib.xml     

在<project name=”hadoopbuildcontrib” xmlns:ivy=”antlib:org.apache.ivy.ant”>下面添加

``` xml
<property name=”eclipse.home” location=”#{你的eclipse安装目录}” /> 
<property name=”version” value=”1.0.4″/>
```

2.修改hadoop/src/contrib/eclipse-plugin/build.xml

1)添加

``` xml
<path id=”hadoop-jars”>
<fileset dir=”${hadoop.root}/”>
<include name=”hadoop-*.jar”/>
</fileset> 
</path>
```

2)在<path id=”classpath”>添加

``` xml
<path id=”classpath”>
<pathelement location=”${build.classes}”/>
<pathelement location=”${hadoop.root}/build/classes”/>
<!– hadoop-core-1.0.4.jar dependency –> 
<pathelement location=”${hadoop.root}”/> 
<!– common lib dependency –> 
<pathelement location=”${hadoop.root}/lib”/>
<path refid=”eclipse-sdk-jars”/>
<path refid=”hadoop-jars”/>
</path>
```

3)在<target name=”jar” depends=”compile” unless=”skip.contrib”>添加

``` xml
<target name=”jar” depends=”compile” unless=”skip.contrib”>
<mkdir dir=”${build.dir}/lib”/>
<!– 将以下jar包打进hadoop-eclipse-1.0.4.jar中 –> 
<copy file=”${hadoop.root}/hadoop-core-1.0.4.jar” tofile=”${build.dir}/lib/hadoop-core.jar” verbose=”true”/> 
<copy file=”${hadoop.root}/lib/commons-cli-1.2.jar” todir=”${build.dir}/lib” verbose=”true”/> 
<copy file=”${hadoop.root}/lib/commons-lang-2.4.jar” todir=”${build.dir}/lib” verbose=”true”/> 
<copy file=”${hadoop.root}/lib/commons-configuration-1.6.jar” todir=”${build.dir}/lib” verbose=”true”/> 
<copy file=”${hadoop.root}/lib/jackson-mapper-asl-1.8.8.jar” todir=”${build.dir}/lib” verbose=”true”/> 
<copy file=”${hadoop.root}/lib/jackson-core-asl-1.8.8.jar” todir=”${build.dir}/lib” verbose=”true”/> 
<copy file=”${hadoop.root}/lib/commons-httpclient-3.0.1.jar” todir=”${build.dir}/lib” verbose=”true”/> 
<jar
jarfile=”${build.dir}/hadoop-${name}-${version}.jar”
manifest=”${root}/META-INF/MANIFEST.MF”>
<fileset dir=”${build.dir}” includes=”classes/ lib/”/>
<fileset dir=”${root}” includes=”resources/ plugin.xml”/>
</jar>
</target>
```

3.将hadoop-core-1.0.4.jar复制到hadoop/build目录下

4.将hadoop/lib/commons-cli-1.2.jar复制到hadoop/build/ivy/lib/Hadoop/common（没有请自行创建）目录下

5.进入hadoop/src/contrib目录，执行ant jar

6.将hadoop/build/contrib/eclipse-plugin/hadoop-eclipse-plugin-1.0.4.jar复制到eclipse/plugins目录下

参考资料：

<http://tianwenbo.iteye.com/blog/1464242>
<http://blog.csdn.net/yundixiaoduo/article/details/7451753>
<http://hi.baidu.com/geogrex/item/4e5853ce8fd4e01f0ad93a9f#0>