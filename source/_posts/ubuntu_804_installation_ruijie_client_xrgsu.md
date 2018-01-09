---
layout: post
title: 'Ubuntu 8.04安装锐捷客户端xrgsu'
date: 2008-11-1
categories: [Linux]
tags: [Ubuntu]
keywords: "Ubuntu"
description: 
comments: true
---

myxrgsu程序下载地址 http://forum.ubuntu.org.cn/download.php?id=16889
libpcap-0.6.2下载地址 http://forum.ubuntu.org.cn/download.php?id=16889
libstdc++.so.5下载地址 http://wchuanye.blogbus.com/files/12172318870.zip

Ubuntu下设置： 
1.拷贝myxrgsu ruijie到/usr/bin目录下，需要root权限 
2.拷贝库文件libpcap.so.0.6.2 libstdc++.so.5到/usr/lib目录下，需要root权限 
3.配置网卡 
  配置接口 
  ```sudo gedit /etc/network/interfaces```
   
  ```auto eth0  //启动以太网卡 
  iface eth0 inet static  //静态IP 
  //iface eth0 inet dhcp  动态分配IP 
  address XXX.XXX.XXX.XXX  //静态IP地址 
  netmask XXX.XXX.XXX.X  //子网掩码 
  gateway XXX.XXX.XXX.XXX //网关地址 ```
  
  配置DNS服务器 
  ```sudo gedit /etc/resolv.conf
  
  nameserver XXX.XXX.XXX.XXX```
  
  修改MAC地址
  ``` bash
  sudo gedit /etc/init.d/rc.local  
  ```
   增加  
  ``` bash
  sudo /sbin/ifconfig eth0 hw ether XX:XX:XX:XX:XX:XX
  sudo /etc/init.d/networking restart```
 
4.重启网络配置 
  ``` bash
  sudo /etc/init.d/networking restart
  ```
  
5.在终端中输入sudo myxrgsu -a，根据提示连接
 
6.可联网后安装expect 
  ``` bash
  sudo apt-get install expect
  ```
  如提示找不到软件包，应更新软件源及软件列表 apt-get update
 
7.修改自动连接脚本 
  ``` bash
  sudo gedit /usr/bin/ruijie
  ```
  加入下面脚本：

``` bash 
#!/usr/bin/expect
#  –by Killua 2008.10.31
set timeout 10
spawn myxrgsu -a
expect “Please input your user name:”
send “XXXXXXXX/r”
expect “Please input your password:”
send “XXXXXX/r”
expect “Use DHCP,1-Use,0-UnUse(Default: 0):”
send “0/r”
expect “Use default auth parameter,0-Use 1-UnUse(Default: 0):”
send “0/r”
sleep .2
set timeout 10
expect “Please input ‘unauth’ to LogOff:”
set timeout 360000
expect “myxrgsu exit!”
sleep .2
send_user “Reconnect please./r/r”
close
#end
```
8.sudo ruijie 即可自动连接 
  如出现错误，可修改自动连接脚本