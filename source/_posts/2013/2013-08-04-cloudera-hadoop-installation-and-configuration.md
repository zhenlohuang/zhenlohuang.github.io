---
layout: post
title: 'Cloudera Hadoop安装及配置'
date: 2013-8-4
wordpress_id: 4065
permalink: /archives/cloudera-hadoop-installation-and-configuration.html
categories: [Big Data]
tags: [Cloudera, Hadoop]
keywords: "Cloudera, Hadoop"
description: 
comments: true
---
# Cloudera Manager安装

## 1）安装Postgresql
具体安装及配置参见：[Postgresql 安装及配置](http://blog.yidooo.net/archives/postgresql-installation-and-configuration.html)

## 修改yum.conf
由于Cloudera的服务器在国外，网络连接较慢，经常在安装过程中包下载失败，进而导致安装失败。

``` bash 
sudo gedit /etc/yum.conf
```

添加*timeout=0*

```
sudo yum update
```

## 3）安装Cloudera Manager

*下载地址：* <https://ccp.cloudera.com/display/SUPPORT/Downloads>

```
sudo +x cloudera-manager-installer.bin
sudo ./cloudera-manager-installer.bin
```
之后一路Next/Yes即可

安装完成试着访问<http://localhost:7180/>, 如没问题说明安装完成


# Cloudera Cluster安装

## 1) 修改/etc/hosts
添加

```
10.182.251.250 zhenlhua-master
10.182.251.239 zhenlhua-slave1
```
## 2）创建无密码SSH链接*
在master机器上执行ssh-keygen -t rsa，在~/.ssh/中生成id_rsa和id_rsa.pub。

将id_rsa.pub拷贝到slave机器,并且将id_rsa.pub的内容添加到~/.ssh/authorized_keys中，修改权限.

```
touch ~/.ssh/authorized_keys
sudo chmod 700  ~/.ssh
sudo chmod 600 ~/.ssh/authorized_keys
```
在master里连接slave

```
ssh zhenlhua-slave1
ssh zhenlhua-slave2
```

## 3）安装Cloudera Cluster

①登录
访问 http://10.182.251.250:7180   
用户名：admin   
密码：admin   
![image](/images/uploads/2013/08/1.png)

②版本选择    
![image](/images/uploads/2013/08/2.png)

③安装组件列表    
![image](/images/uploads/2013/08/3.png)

④指定主机名    
![image](/images/uploads/2013/08/4.png)

⑤选择软件安装方式    
![image](/images/uploads/2013/08/6.png)

⑥集群安装    
![image](/images/uploads/2013/08/7.png)

⑦检查主机正确性    
![image](/images/uploads/2013/08/8.png)

⑧安装CDH4服务    
![image](/images/uploads/2013/08/10.png)

⑨安装数据库    
![image](/images/uploads/2013/08/11.png)

⑩适合配置    
![image](/images/uploads/2013/08/12.png)