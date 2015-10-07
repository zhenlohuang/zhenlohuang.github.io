---
layout: post
title: '基于贝叶斯推断的拼写检查'
date: 2013-4-15
wordpress_id: 3951
permalink: /archives/spelling-corrector-based-on-bayesian-inference.html
categories: [Data Mining]
tags: [Bayesian]
keywords: "Bayesian"
description: 
comments: true
---
# 拼写检查
所谓拼写检查，就是在使用Google的时候，输入错误的时候，系统对其自动纠正，如下图所示：

![image](/images/uploads/2013/04/Selection_001.png)

目前有很多方法可以实现这样的技术，这里使用的是贝叶斯推断的方法。

# 贝叶斯推断
有关贝叶斯推断的相关理论和背景知识，可以参考<href="http://www.ruanyifeng.com/blog/2011/08/bayesian_inference_part_one.html>贝叶斯推断及其互联网应用（一）：定理简介</a>

# 原理
用户输入了一个单词。这时分成两种情况：拼写正确，或者拼写不正确。我们把拼写正确的情况记做c（代表correct），拼写错误的情况记做w（代表wrong）。

拼写检查，就是在发生w的情况下，试图推断出c。从概率论的角度看，就是已知w，然后在若干个备选方案中，找出可能性最大的那个c，也就是求P(c|w)的最大值。根据贝叶斯定理：P(c|w) = P(w|c) * P(c) / P(w)对于所有备选的c来说，对应的都是同一个w，所以它们的P(w)是相同的，因此我们求的其实是P(w|c) * P(c)的最大值。

P(c)的含义是，某个正确的词的出现"概率"，它可以用"频率"代替。如果我们有一个足够大的文本库，那么这个文本库中每个单词的出现频率，就相当于它的发生概率。某个词的出现频率越高，P(c)就越大。

P(w|c)的含义是，在试图拼写c的情况下，出现拼写错误w的概率。这需要统计数据的支持，但是为了简化问题，我们假设两个单词在字形上越接近，就有越可能拼错，P(w|C)就越大。举例来说，相差一个字母的拼法，就比相差两个字母的拼法，发生概率更高。你想拼写单词hello，那么错误拼成hallo（相差一个字母）的可能性，就比拼成haallo高（相差两个字母）。

所以，我们只要找到与输入单词在字形上最相近的那些词，再在其中挑出出现频率最高的一个，就能实现 P(w|c) * P(c) 的最大值。

# 算法
## 1）语料库读入
这里使用的是<http://en.wiktionary.org/wiki/Wiktionary:Frequency_lists#English>语料库，该语料库是这是一个由志愿者编纂的多语言词典计划，它旨在囊括各种语言词汇的语源、读音和解释。任何人甚至无须登录就可以编辑任何字词。在源代码中使用的wiktionary文件，里面每一行为一个单词以及单词的频率，格式为：word:frequency。

``` python 
DICTIONARY = {}

def read_words():
    '''
    Read bag of words from file
    '''
    f = open('wiktionary', 'r')
    for line in f.readlines():
        DICTIONARY.update({line.split(':')[0] : float(line.split(':')[1])})
```

## 2）计算相似单词
这里计算相似单词使用的是编辑距离的方法，所谓编辑距离，两个单词通过删除、交换、更改和插入四种操作中的一种，就可以让一个词变成另一个词。这里生成编辑距离为1的单词集

``` python 
def generate_edit_distance1_words(word):
    '''
    Generate a set of all words that are one edit distance from word
    '''
    #delete a character for word
    deletes = [word[1:]]
    deletes += [str(word[:i] + word[i+1:]) for i in range(1, len(word))]
    #change position between two character for word
    transposes = [str(word[1] + word[0] + word[2:])]
    transposes += [str(word[:i-1] + word[i] + word[i-1] + word[i+1:]) for i in range(2, len(word))]
    #replaces one character
    replaces = [str(c + word[1:]) for c in string.lowercase]
    replaces += [str(word[:i] + c + word[i+1:]) for i in range(1, len(word)) for c in string.lowercase]
    #insert one character
    inserts = [str(c + word) for c in string.lowercase]
    inserts += [str(word[:i] + c + word[i:]) for i in range(1, len(word)) for c in string.lowercase]
    inserts += [str(word + c) for c in string.lowercase]

    return set(deletes + transposes + replaces +inserts)
```

## 3）单词修正
根据step 2所得到的候选单词集，在字典中进行比较，得到概率最大作为拼写建议。

``` python 
def words_filter(words):
    '''
    Word filter
    '''    
    return set(word for word in words if word in DICTIONARY.keys())

def candidates(word):
    '''
    Get all candidates for word
    '''
    if word in DICTIONARY.keys():
        return set([word])
    else:
        return words_filter([word]) | words_filter(generate_edit_distance1_words(word)) 

def correct(word):
    '''
    correct the word.
    '''
    candidate_words = candidates(word)
    candidate_dict = {}
    for item in candidate_words:
        candidate_dict.setdefault(item, 0)
        candidate_dict.update({item : DICTIONARY[item]})

    return max(candidate_dict, key = lambda x : candidate_dict[x])
```

# 示例

``` bash
python spelling_corrector.py pragramming
pragramming -> programming
```

# 完整代码

``` python 
#!/usr/bin/env python

import string
import os
import sys

DICTIONARY = {}

def read_words():
    '''
    Read bag of words from file
    '''
    f = open('wiktionary', 'r')
    for line in f.readlines():
        DICTIONARY.update({line.split(':')[0] : float(line.split(':')[1])})

def generate_edit_distance1_words(word):
    '''
    Generate a set of all words that are one edit distance from word
    '''
    #delete a character for word
    deletes = [word[1:]]
    deletes += [str(word[:i] + word[i+1:]) for i in range(1, len(word))]
    #change position between two character for word
    transposes = [str(word[1] + word[0] + word[2:])]
    transposes += [str(word[:i-1] + word[i] + word[i-1] + word[i+1:]) for i in range(2, len(word))]
    #replaces one character
    replaces = [str(c + word[1:]) for c in string.lowercase]
    replaces += [str(word[:i] + c + word[i+1:]) for i in range(1, len(word)) for c in string.lowercase]
    #insert one character
    inserts = [str(c + word) for c in string.lowercase]
    inserts += [str(word[:i] + c + word[i:]) for i in range(1, len(word)) for c in string.lowercase]
    inserts += [str(word + c) for c in string.lowercase]

    return set(deletes + transposes + replaces +inserts)

def words_filter(words):
    '''
    Word filter
    '''    
    return set(word for word in words if word in DICTIONARY.keys())

def candidates(word):
    '''
    Get all candidates for word
    '''
    if word in DICTIONARY.keys():
        return set([word])
    else:
        return words_filter([word]) | words_filter(generate_edit_distance1_words(word)) 

def correct(word):
    '''
    correct the word.
    '''
    candidate_words = candidates(word)
    candidate_dict = {}
    for item in candidate_words:
        candidate_dict.setdefault(item, 0)
        candidate_dict.update({item : DICTIONARY[item]})

    return max(candidate_dict, key = lambda x : candidate_dict[x])

if __name__ == '__main__':
    read_words()
    print sys.argv[1], '->', correct(sys.argv[1])
```
**完整代码可以参见github：**<https://github.com/killuahzl/spelling_corrector>

不足之处    
拼写检查的精度很大程度依赖所使用的语料库，而且本文仅仅只是抛砖引玉，只考虑编辑距离为1的单词的情况。许多情况下单词的拼写错误不只一处。

# 参考资料
<http://norvig.com/spell-correct.html>

<http://www.ruanyifeng.com/blog/2012/10/spelling_corrector.html>

<http://en.wiktionary.org/wiki/Wiktionary:Frequency_lists#English>

<http://zh.wiktionary.org/zh/Wiktionary:%E9%A6%96%E9%A1%B5>
