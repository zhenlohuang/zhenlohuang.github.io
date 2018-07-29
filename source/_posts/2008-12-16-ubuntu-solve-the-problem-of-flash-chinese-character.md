---
layout: post
title: 'Ubuntu 解决flash方块字问题'
date: 2008-12-16
categories: [Linux]
tags: [Ubuntu, Flash]
keywords: "Ubuntu, Flash"
description: 
comments: true
---

``` bash
sudo cp /etc/fonts/conf.d/49-sansserif.conf /etc/fonts/conf.d/49-sansserif.conf_bak (备份)
sudo gedit /etc/fonts/conf.d/49-sansserif.conf
```
将倒数第四行改为自己的字体即可。

``` xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
<!--
  If the font still has no generic name, add sans-serif
 -->
    <match target="pattern">
        <test qual="all" name="family" compare="not_eq">
            <string>sans-serif</string>
        </test>
        <test qual="all" name="family" compare="not_eq">
            <string>serif</string>
        </test>
        <test qual="all" name="family" compare="not_eq">
            <string>monospace</string>
        </test>
        <edit name="family" mode="append_last">
            <string>文泉驿正黑</string>
        </edit>
    </match>
</fontconfig>
```