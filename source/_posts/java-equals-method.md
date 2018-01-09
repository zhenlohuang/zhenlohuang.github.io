---
layout: post
title: 'Java Equals方法完美实现'
date: 2013-5-16
categories: [Programming, Java]
tags: [Java]
keywords: "Java"
description: 
comments: true
---
# Equals：
Object类中的equals方法用于检测一个对象是否等于另一个对象。

# 实现：

``` java 
public class MyClass {

	private Integer field1;

	private String field2;

	@Override
	public boolean equals(Object otherObject) {

		// 判断是否引用同一个对象
		if (this == otherObject) {
			return true;
		}

		// 检测otherObject是否为空
		if (otherObject == null) {
			return false;
		}

		// 比较this和otherObject是不是属于同一个类
		if (!(otherObject instanceof MyClass)) {
			return false;
		}

		// 比较每个域是否相等
		MyClass other = (MyClass) otherObject;
		return this.field1.equals(other.field1)
				&& this.field2.equals(other.field2);
	}
}
```
