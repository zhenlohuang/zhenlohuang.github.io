---
layout: post
title: 'Fedora TexLive安装及中文环境配置'
date: 2012-2-7
wordpress_id: 538
categories: [Linux]
tags: [Fedora]
keywords: "Fedora"
description: 
comments: true
---

*OS：Fedora 16
TexLive Version: TexLive 2011

1)添加rpm源*

```
sudo rpm -i http://jnovy.fedorapeople.org/texlive/2011/packages.fc16/texlive-release.noarch.rpm
```
（其他版本可以到http://jnovy.fedorapeople.org/找下对应源）

*2）安装texlive2011*

```
sudo yum clean all
sudo yum install texlive
```

*3)下载中文库包*
Url：http://bj.soulinfo.com/~hugang/tex/tex2007/YueWang-zhfonts-final_1.01.tar.bz2    
解压后取出texmf-var，将里面的内容分别复制到/usr/share/texlive/texmf-local和/usr/share/texlive/texmf-var里面    
然后执行sudo texhash,重新建立数据库    

*4）测试*
documentclass{article}    
usepackage{CJKutf8}    
begin{document}    
begin{CJK}{UTF8}{hei}    
Hello , Latex !    
你好，Latex    
end{CJK}    
end{document}    

保存test.tex退出,然后，执行    
latex test.tex   
dvipdfm test.dvi    


PS：如果运行时出现CJKutf8.sty找不到，执行sudo yum install 'tex(CJKutf8.sty)'解决

