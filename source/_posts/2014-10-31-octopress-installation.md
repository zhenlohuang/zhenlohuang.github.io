---
layout: post
title: "Octopress-installation"
date: 2014-10-31
categories: [Others]
tags: [Octopress]
keywords: "Octopress"
description: 
comments: true
---

Octopress是一个基于Jekyll静态页面生成器，多数用于Github Pages的生成。本文主要介绍Octopress在Windows以及Linux平台下的安装，本文的安装步骤基于Window 7以及Ubuntu 14.04。

# Git安装

Git下载地址：<http://git-scm.com/downloads>

Ubuntu用户也可执行下面命令：

``` bash
sudo apt-get install git
```

# Ruby安装

Ruby下载地址:<http://rubyinstaller.org/downloads/>。这里需要注意，所选择的版本应为Ruby1.9.3或者更高，本文使用的是Ruby1.9.3。

Windows用户还需额外安装RubyDevKit，下载地址:<http://rubyinstaller.org/downloads/>

``` bash
cd C:/RubyDevKit
ruby dk.rb init
ruby dk.rb install
```

安装后使用```ruby --version```命令确认所安装的Ruby版本。

# Octopress安装 
克隆Octopress源码，安装依赖。

``` bash
git clone git://github.com/imathis/octopress.git octopress
cd octopress
gem install bundler
bundle install
```

安装Octopress默认主题。

``` bash
rake install
```

到目前为止，Octopress已经安装完成，可以使用```rake generate```命令尝试生成静态页面。使用```rake preview```命令到<http://localhost:4000>进行预览。

# Troubleshooting

Windows用户，在执行```rake generate```命令的过程中，可能会出现类似

```
warning: cannot close fd before spawn
```

的警告信息，导致静态页面生成失败。这是主要是由于pygments的版本与Windows不兼容导致的。可以尝试执行以下步骤进行修复：

``` bash
gem uninstall pygments.rb --version ">0.5.0"
gem install pygments.rb --version "=0.5.0"
```
在项目文件夹下修改Gemfile.lock文件

```
pygments.rb (0.6.0) => pygments.rb (0.5.0)
```

