---
title: Kerberos认证流程
date: 2019-10-11 02:32:22
tags: Kerberos
---

# 术语解释
* AD: Active Directory。
* Service Session Key: 服务会话密钥。
* Logon Session Key: 登录会话密钥。
* KDC: Key Distribution Center。
* KAS: Key Kerberos Authentication Service。它是KDC的一个服务。
* TGS: Ticket Granting Service；它是KDC的一个服务。
* Service Ticket: 服务票据，通过TGS获取，主要包括用户信息与Service Session Key。
* TGT: Ticket  Granting Ticket。通过KAS获取，主要包括用户信息与Logon Session Key。
* Authenticator: 交互双方预先知晓的信息，通过它来对对方做认证。

# KDC整体结构图
![kerberos_architecture.jpg](/images/kerberos_architecture.jpg)

# Kerberos认证的流程
1. 客户端通过KDC的KAS服务获取TGT。
2. 通过TGT获取Service Ticket。
3. 通过Service Ticket访问服务资源 。

# Client与Server之间的消息交互
1. 客户端发送消息到KDC的KAS服务一获取TGT
![kerberos_step_1.jpg](/images/kerberos_step_1.jpg)
2. KAS 发送信息到Client
![kerberos_step_2.jpg](/images/kerberos_step_2.jpg)
3. 客户端发送消息到KDC（Key Distribution Center）的TGS（Ticket Granting Service）服务一获取ST（Service Ticket）
![kerberos_step_3.jpg](/images/kerberos_step_3.jpg)
4. TGS服务返回消息给客户端
![kerberos_step_4.jpg](/images/kerberos_step_4.jpg)
5. 客户端通过ST访问服务器资源
![kerberos_step_5.jpg](/images/kerberos_step_5.jpg)
6. 客户端对服务器的认证
![kerberos_step_6.jpg](/images/kerberos_step_6.jpg)
