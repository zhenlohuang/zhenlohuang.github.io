---
layout: post
title: 'Java查看系统默认字符集编码'
date: 2012-12-16
categories: [Programming, Java]
tags: [Java]
keywords: "Java"
description: 
comments: true
---

``` java 
public class EchoDefaultSystemEncoding 
{
    public static void main(String[] args) 
    {
           String encoding = System.getProperty("file.encoding");
           System.out.println("Default System Encoding:" + encoding);
    }
}
```
