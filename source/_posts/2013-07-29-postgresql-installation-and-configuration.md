---
layout: post
title: 'Postgresql 安装及配置'
date: 2013-7-29
categories: [Database, PostgreSQL]
tags: [PostgreSQL]
keywords: "PostgreSQL"
description: 
comments: true
---
**安装**

``` bash 
sudo yum install postgresql postgresql-server
```

**初始化数据库**

``` bash 
  service postgresql initdb
```


**配置postgresql.conf**

``` bash 
sudo vi /var/lib/pgsql/data/postgresql.conf
```

```
#---------------------------------------------------------------------------
# CONNECTIONS AND AUTHENTICATION
#---------------------------------------------------------------------------
listen_addresses = '*'              # what IP address(es) to listen on;
                                        # comma-separated list of addresses;
                                        # defaults to 'localhost', '*' = all
                                        # (change requires restart)
port = 5432                          # (change requires restart)
```

**配置pg_hba.conf**

``` bash 
sudo vi /var/lib/pgsql/data/pg_hba.conf
```

```
# TYPE  DATABASE    USER        CIDR-ADDRESS          METHOD
# "local" is for Unix domain socket connections only
local   all         all                               trust
# IPv4 local connections:
host    all         all         127.0.0.1/32          md5
host    all         all         192.168.137.2/16        md5 
host    all         all         192.168.137.3/16        md5 
# IPv6 local connections:
host    all         all         ::1/128               md5 
```

**重启服务**

``` bash 
sudo service postgresql restart
```

