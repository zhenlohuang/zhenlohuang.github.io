---
layout: post
title: 'Ubuntu 软件包备份与清理'
date: 2009-5-22
wordpress_id: 301
categories: [Linux]
tags: [Ubuntu]
keywords: "Ubuntu"
description: 
comments: true
---

# 备份

``` bash
sudo tar cizvf backup.tar.gz /var/cache/apt/archives --exclude=/var/cache/apt/archives/partial/* --exclude=/var/cache/apt/archives/lock
```
# 清理

``` bash
sudo apt-get clean
sudo rm -rf ~/.thumbnails/fail/gnome-thumbnail-factory/*
```

# 还原

``` bash
sudo apt-get update 
sudo tar xzvf backup.tar.gz -C /
```
