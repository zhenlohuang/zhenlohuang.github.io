---
layout: post
title: '编辑距离算法 Levenshtein Distance'
date: 2012-1-8
categories: [Algorithm]
tags: []
keywords: ""
description: 
comments: true
---
编辑距离，又称Levenshtein距离（也叫做EditDistance），是指两个字串之间，由一个转成另一个所需的最少编辑操作次数，如果它们的距离越大，说明它们越是不同。许可的编辑操作包括将一个字符替换成另一个字符，插入一个字符，删除一个字符。<span style="font-family: verdana,'ms song',宋体,Arial,微软雅黑,Helvetica,sans-serif; line-height: 18px; background-color: #f5fafe;">俄罗斯科学家Vladimir Levenshtein在1965年提出这个概念。因此也叫Levenshtein Distance，常用来衡量字符串相似度。

【算法过程】

``` 
int LevenshteinDistance(char s[1..m], char t[1..n])
{
  // for all i and j, d[i,j] will hold the Levenshtein distance between
  // the first i characters of s and the first j characters of t;
  // note that d has (m+1)x(n+1) values
  declare int d[0..m, 0..n]

  for i from 0 to m
    d[i, 0] := i // the distance of any first string to an empty second string
  for j from 0 to n
    d[0, j] := j // the distance of any second string to an empty first string

  for j from 1 to n
  {
    for i from 1 to m
    {
      if s[i] = t[j] then  
        d[i, j] := d[i-1, j-1]       // no operation required
      else
        d[i, j] := minimum
                   (
                     d[i-1, j] + 1,  // a deletion
                     d[i, j-1] + 1,  // an insertion
                     d[i-1, j-1] + 1 // a substitution
                   )
    }
  }

  return d[m,n]
}
```


【代码】    

``` python 
#Levenshtein Distance Algorithm
#OS : Windows 7
#Python Version : Python 3.2
#IDE : VS2010 + PTVS

def levenshtein(str1, str2):
    """
    Levenshtein Distance Algorithm
    @param str1 string 
    @param str2 string
    @return distance int
    """
    #initialize dist
    dist = [[0 for j in range(len(str2) + 2)] for i in range(len(str1) + 2)]
    for i in range(len(str1) + 1):
        dist[i + 1][0] = i
    for j in range(len(str2) + 1):
        dist[0][j + 1] = j
    for i in range(len(str1)):
        for j in range(len(str2)):
            if str1[i] == str2[j]:
                dist[i+1][j+1] = dist[i][j]
            else:
                dist[i+1][j+1] = min(dist[i][j+1], dist[i+1][j], dist[i][j]) + 1
    return dist[len(str1)][len(str2)]

def main():
    str1 = 'sitting'
    str2 = 'kitten'
    leven_dist = levenshtein(str1, str2)
    print("str1=%s, str2=%s, distance=%d" % (str1, str2, leven_dist))

if __name__ == "__main__":
    main()
```

【参考资料】
<http://en.wikipedia.org/wiki/Levenshtein_distance>
