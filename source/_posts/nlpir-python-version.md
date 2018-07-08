---
layout: post
title: 'NLPIR（ICTCLAS 2013）分词工具Python封装'
date: 2013-4-19
categories: [NLP]
tags: [Python, NLPIR]
keywords: "Python, NLPIR"
description: 
comments: true
---
**本文只适用于python-nlpir V1.0版本，有关V2.0版本详情参照项目源代码。**

python-nlpir是NLPIR中文分词工具的Python封装，利用SWIG完成C++到python的接口转换。

*项目地址：*<https://github.com/kevinxhuang/python-nlpir>

NLPIR汉语分词系统（又名ICTCLAS2013），主要功能包括中文分词；词性标注；命名实体识别；用户词典功能；支持GBK编码、UTF8编码、BIG5编码。新增微博分词、新词发现与关键词提取；是当前最好的中文分词工具之一。
# 1、使用SWIG进行接口转换
SWIG是个帮助使用C或者C++编写的软件能与其它各种高级编程语言进行嵌入联接的开发工具。SWIG能应用于各种不同类型的语言包括常用脚本编译语言例如Perl, PHP, Python, Tcl, Ruby and PHP。支持语言列表中也包括非脚本编译语言，例如C#, Common Lisp (CLISP, Allegro CL, CFFI, UFFI), Java, Modula-3, OCAML以及R，甚至是编译器或者汇编的计划应用（Guile, MzScheme, Chicken）。SWIG普遍应用于创建高级语言解析或汇编程序环境，用户接口，作为一种用来测试C/C++或进行原型设计的工具。SWIG还能够导出XML或Lisp s-expressions格式的解析树。SWIG可以被自由使用，发布，修改用于商业或非商业中。

## 1）编写interface文件

``` cpp 
%module NLPIR
%{
#define SWIG_FILE_WITH_INIT
#include "NLPIR.h"
%}
#define POS_MAP_NUMBER 4
#define ICT_POS_MAP_FIRST 1
#define ICT_POS_MAP_SECOND 0
#define PKU_POS_MAP_SECOND 2
#define PKU_POS_MAP_FIRST 3
#define POS_SIZE 40
#define GBK_CODE 0
#define UTF8_CODE GBK_CODE+1
#define BIG5_CODE GBK_CODE+2
#define GBK_FANTI_CODE GBK_CODE+3

bool NLPIR_Init(const char * sDataPath=0,int encode=GBK_CODE,const char*sLicenceCode=0);
bool NLPIR_Exit();
const char * NLPIR_ParagraphProcess(const char *sParagraph,int bPOStagged=1);
const result_t * NLPIR_ParagraphProcessA(const char *sParagraph,int *pResultCount,bool bUserDict=true);
int NLPIR_GetParagraphProcessAWordCount(const char *sParagraph);
void NLPIR_ParagraphProcessAW(int nCount,result_t * result);
double NLPIR_FileProcess(const char *sSourceFilename,const char *sResultFilename,int bPOStagged=1);
unsigned int NLPIR_ImportUserDict(const char *sFilename);
int NLPIR_AddUserWord(const char *sWord);//add by qp 2008.11.10
int NLPIR_SaveTheUsrDic();
int NLPIR_DelUsrWord(const char *sWord);
double NLPIR_GetUniProb(const char *sWord);
bool NLPIR_IsWord(const char *sWord);
const char * NLPIR_GetKeyWords(const char *sLine,int nMaxKeyLimit=50,bool bWeightOut=false);
const char * NLPIR_GetFileKeyWords(const char *sFilename,int nMaxKeyLimit=50,bool bWeightOut=false);
const char * NLPIR_GetNewWords(const char *sLine,int nMaxKeyLimit=50,bool bWeightOut=false);
const char * NLPIR_GetFileNewWords(const char *sFilename,int nMaxKeyLimit=50,bool bWeightOut=false);
unsigned long NLPIR_FingerPrint(const char *sLine);
int NLPIR_SetPOSmap(int nPOSmap);
bool NLPIR_NWI_Start();//New Word Indentification Start
int NLPIR_NWI_AddFile(const char *sFilename);
bool NLPIR_NWI_AddMem(const char *sText);
bool NLPIR_NWI_Complete();
const char * NLPIR_NWI_GetResult(bool bWeightOut=false);
unsigned int NLPIR_NWI_Result2UserDict();
```

## 2）接口转换

``` bash
swig -c++ -python NLPIR.interface
```
执行完成接口转换后，将生成NLPIR_wrap.cxx、NLPIR.py两个文件，到目前为止已经完成了接口的转换，不过现在接口还不能使用，因为还没有编译好相应的库。

# 2、编写setup.py文件

``` python 
'''
setup.py file for NLPIR
'''
from distutils.core import setup, Extension

NLPIR_module = Extension('_NLPIR',sources=['NLPIR_wrap.cxx'], libraries = ['NLPIR'])

setup(name = 'NLPIR',
    version = '1.1',
    author = 'Killua',
    description = 'Python for NLPIR',
    ext_modules = [NLPIR_module],
    py_modules = ['NLPIR'],
    )
```

# 3、编译代码

## 1）复制libNLPIR.so到/usr/lib目录下

``` bash
sudo cp libNLPIR.so /usr/lib
```

## 2）修改NLPIR.h文件
由于原来的NLPIR.h在gcc编译器中的兼容性并不好，所以需要对原来的头文件进行简单的修改。   
将

``` cpp
#ifdef OS_LINUX
	#define NLPIR_API 
#else
#ifdef NLPIR_EXPORTS
#ifdef CSHARP_API
#define NLPIR_API extern "C" __declspec(dllexport)
#else
#define NLPIR_API __declspec(dllexport)
#endif
#else
#ifdef CSHARP_API
#define NLPIR_API extern "C"  __declspec(dllimport)
#else
#define NLPIR_API __declspec(dllimport)
#endif
#endif
#endif
#if defined(NLPIR_JNI_EXPORTS)||defined(KEYEXTRACT_EXPORTS)
	#define NLPIR_API 
#endif
```
改为

``` cpp
#ifdef linux
    #define NLPIR_API 
#elif _WIN32 || _WIN64
    #ifdef NLPIR_EXPORTS
        #define NLPIR_API __declspec(dllexport)
    #else
        #define NLPIR_API __declspec(dllimport)
    #endif
#endif
```
将

``` cpp
#ifdef OS_LINUX
class  CNLPIR {
#else
class  __declspec(dllexport) CNLPIR {
#endif
```
改为

``` cpp
#ifdef linux
class  CNLPIR {
#else
class  __declspec(dllexport) CNLPIR {
#endif
```

## 3）编译代码

``` python 
python setup.py build_ext --inplace
```

# 4、编写PyNLPIR.py
为了方便使用对生成的NLPIR.py进行重封装。

``` python 
#!/usr/bin/env python
#coding:utf-8
'''
Created on Fri 19, 2013

@author: killua
@e-mail: killua_hzl@163.com
@Decription: Python for NLPIR
'''
import NLPIR
import os

def nlpir_init(init_dir = '.', code_type = 'GBK'):
    '''
    Init the analyzer and prepare necessary data for NLPIR according the configure file.
    '''
    if code_type == 'GBK':
        is_succeed = NLPIR.NLPIR_Init(init_dir, NLPIR.GBK_CODE)
    elif code_type == 'UTF-8' or code_type == 'UTF8':
        is_succeed = NLPIR.NLPIR_Init(init_dir, NLPIR.UTF8_CODE)
    elif code_type == 'BIG5':
        is_succeed = NLPIR.NLPIR_Init(init_dir, NLPIR.BIG5_CODE)
    elif code_type == 'GBK_FANTI':
        is_succeed = NLPIR.NLPIR_Init(init_dir, NLPIR.GBK_FANTI_CODE)
    if is_succeed:
        print 'NLPIR Successful.'
    else:
        print 'NLPIR Failed.'

def nlpir_exit():
    '''
    Exit the program and free all resources and destroy all working buffer used in NLPIR.    
    '''
    return NLPIR.NLPIR_Exit()

def nlpir_import_user_dict(user_dict):
    '''
    Import user-defined dictionary from a text file. 
    '''
    return NLPIR.NLPIR_ImportUserDict(user_dict)

def nlpir_paragraph_process(text, is_pos_tagged = False):
    '''
    Process a paragraph
    '''
    return NLPIR.NLPIR_ParagraphProcess(text, is_pos_tagged)

def nlpir_file_process(source_file, target_file, is_pos_tagged = False):
    '''
    Process a text file
    '''
    return NLPIR.NLPIR_FileProcess(source_file, target_file, is_pos_tagged)

def nlpir_add_user_word(word):
    '''
    Add a word to the user dictionary.
    '''
    return NLPIR.NLPIR_AddUserWord(word)

def nlpir_save_user_dict():
    '''
    Save the user dictionary to disk.
    '''
    return NLPIR.NLPIR_SaveTheUsrDic()

def nlpir_delete_user_word(word):
    '''
    Delete a word from the  user dictionary.
    '''
    return NLPIR.NLPIR_DelUsrWord(word)
```

# 测试
## 编写demo.py

``` python 
#!/usr/bin/env python
#encoding: utf8

from PyNLPIR import *

if __name__ == '__main__':

    nlpir_init('.', 'UTF-8')	
    print nlpir_paragraph_process(r'@ICTCLAS张华平博士 应各位ICTCLAS用户的要求，张华平博士提前发布ICTCLAS2013 版本，为了与以前工作进行大的区隔，并推广NLPIR自然语言处理与信息检索共享平台，从本版本开始，系统名称调整为NLPIR汉语分词系统。')
    print 
    print nlpir_paragraph_process(r'“屌丝”这个嘲讽意味的代词迅速爆红，迎合了大众的心理和趣味。因为你会发现从表面符合屌丝定义的人，到和屌丝属性八竿子打不着的人，都在争相认领这一名号。当人人都在忙着确认自己的屌丝身份，并乐此不疲时，屌丝一词一定与时代的什么特征实现了合拍。“屌丝”不是阿Q，他们公然比惨并乐在其中有评论认为，“屌丝”是新时代的阿Q，两者并不完全相同。首先，阿Q是文学巨匠鲁迅一己之力创造的，而“屌丝”则是网络群体狂欢的结果，它是真正由网民集体创作的形象；另外，阿Q最重要的特征是“精神胜利法”，梦想的是“银盔银甲”，意淫的是“我手持钢鞭将你打”。', True)
    nlpir_exit()
```
## 运行结果
![image](/images/legacy/2013/04/nlpir.png)

# 常见问题
- “Unable to find vcvarsall.bat”解决办法

参见：<http://www.yidooo.net/archives/unable-to-find-vcvarsall-bat-solution.html>

- 提示找不到_NLPIR模块提示找不到_NLPIR模块的文件

Windows：将NLPIR.dll copy到System32下

Linux：将libNLPIR.so copy到/usr/lib或者/usr/lib64下

- 提示License过期 

请参考：<http://ictclas.nlpir.org/newsDetail?DocId=386>

# Change Log
*Date: 2013-11-15*
- 添加对Window 64bit和Linux 64bit的支持。
- 对项目文件结构进行调整，将library文件进行分离。
- 将NLPIR升级到最新版本。

# 感谢
Version 1.1: 感谢[zzdwcm](https://github.com/zzdwcm)，所提供有关Linux 64bit的补丁。

# 参考资料
<http://ictclas.nlpir.org/>

<http://www.nilday.com/nlpirictclas2013-python%E7%89%88/>

<https://github.com/ch0psticks/nlpir-python-win32>