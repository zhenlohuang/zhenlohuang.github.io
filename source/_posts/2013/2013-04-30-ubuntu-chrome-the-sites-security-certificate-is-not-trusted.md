---
layout: post
title: 'Ubuntu GoAgent: "the site security certificate is not trusted"解决办法'
date: 2013-4-30
wordpress_id: 3997
permalink: /archives/ubuntu-chrome-the-sites-security-certificate-is-not-trusted.html
categories: [Others]
tags: [GoAgent]
keywords: "GoAgent"
description: 
comments: true
---

今天在Ubuntu下面安装GoAgent遇到出现“the site"s security certificate is not trusted”的问题，根据提示发现是证书错误造成的，解决方法如下。

1）安装libnss3-tools

```
sudo apt-get install libnss3-tools
```

2）导入证书

```
certutil -d sql:$HOME/.pki/nssdb -A -t TC -n "goagent" -i your_goagent_directory/local/CA.crt
```

参考资料：
<http://nigelzeng.iteye.com/blog/1748283>
