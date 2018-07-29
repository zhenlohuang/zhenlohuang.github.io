---
layout: post
title: 'ORACLE SQL 前端补“0”'
date: 2011-11-27
categories: [Database, Oracle]
tags: [Oracle]
keywords: "Oracle"
description: 
comments: true
---
1）LPAD方法：

``` sql
SELECT LPAD(sal,8,'0') FROM
```
2）TO_CHAR 方法

``` sql
SELECT TO_CHAR(sal,'00000000') From
```
3）SUBSTR方法

``` sql
SELECT SUBSTR('00000000'||sal,-8) FROM
```
===================================================================
补充：
>LPAD和RPAD用法：
>Lpad()函数的用法：
>lpad函数将左边的字符串填充一些特定的字符其语法格式如下：
>lpad(string,n,[pad_string])
>string：可是字符或者参数
>n：字符的长度，是返回的字符串的数量，如果这个数量比原字符串的长度要短，lpad函数将会把字符串截取成从左到右的n个字符;
>pad_string：是个可选参数，这个字符串是要粘贴到string的左边，如果这个参数未写，lpad函数将会在string的左边粘贴空格。
>例如：
>lpad('tech', 7); 将返回' tech'
>lpad('tech', 2); 将返回'te'
>lpad('tech', 8, '0'); 将返回'0000tech'
>lpad('tech on the net', 15, 'z'); 将返回 'tech on the net'
>lpad('tech on the net', 16, 'z'); 将返回 'ztech on the net'

>Rpad()函数的用法：
>rpad函数将右边的字符串填充一些特定的字符其语法格式如下：
>rpad(string,n,[pad_string])
>string：可是字符或者参数
>n：字符的长度，是返回的字符串的数量，如果这个数量比原字符串的长度要短，lpad函数将会把字符串截取成从左到右的n个字符;
>pad_string：是个可选参数，这个字符串是要粘贴到string的右边，如果这个参数未写，lpad函数将会在string的右边粘贴空格。
>例如：
>rpad('tech', 7); 将返回' tech'
>rpad('tech', 2); 将返回'te'
>rpad('tech', 8, '0'); 将返回'tech0000'
>rpad('tech on the net', 15, 'z'); 将返回 'tech on the net'
>rpad('tech on the net', 16, 'z'); 将返回 'tech on the netz'

参考资料：<http://hi.baidu.com/ljw460/blog/item/5788594a1b55ff2608f7efc5.html>
