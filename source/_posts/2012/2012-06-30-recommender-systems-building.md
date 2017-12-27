---
layout: post
title: '【集体智慧编程 学习笔记】 推荐系统构建'
date: 2012-6-30
wordpress_id: 3215
permalink: /archives/recommender-systems-building.html
categories: [Data Mining]
tags: []
keywords: ""
description: 
comments: true
---
本文构建了一个简单的推荐系统，使用的数据是真实的数据，叫作MovieLens，来自University of Minnesota‘s GroupLens项目组。代码以Python作为实现语言，使用版本为Python2.7。

loadMovieData：用于数据的读取。userData指的是以userId为键构建的电影评分列表。movieData值的是以movieId为键构建的电影评分列表。

euclidean：用于计算Eucidean距离系数

pearson:用于计算Pearson相关系数

getSimilarItems：计算出movieData中每一项相似度最大的n项

getRecommendationsItems：对于某个user取得推荐结果

代码如下：

``` python
'''
Created on Jun 30, 2012

@Author: killua
@E-mail: killua_hzl@163.com
@Homepage: http://www.yidooo.net

Data set download from : http://www.grouplens.org/system/files/ml-100k.zip

MovieLens data sets were collected by the GroupLens Research Project
at the University of Minnesota.The data was collected through the MovieLens web site
(movielens.umn.edu) during the seven-month period from September 19th, 
1997 through April 22nd, 1998.

This data set consists of:
    * 100,000 ratings (1-5) from 943 users on 1682 movies. 
    * Each user has rated at least 20 movies. 
    * Simple demographic info for the users 

u.data     -- The full u data set, 100000 ratings by 943 users on 1682 items.
              Each user has rated at least 20 movies.  Users and items are
              numbered consecutively from 1.  The data is randomly
              ordered. This is a tab separated list of 
              user id | item id | rating | timestamp. 
              The time stamps are unix seconds since 1/1/1970 UTC    
u.item     -- Information about the items (movies); this is a tab separated
              list of
              movie id | movie title | release date | video release date |
              IMDb URL | unknown | Action | Adventure | Animation |
              Children's | Comedy | Crime | Documentary | Drama | Fantasy |
              Film-Noir | Horror | Musical | Mystery | Romance | Sci-Fi |
              Thriller | War | Western |
              The last 19 fields are the genres, a 1 indicates the movie
              is of that genre, a 0 indicates it is not; movies can be in
              several genres at once.
              The movie ids are the ones used in the u.data data set.              
'''

from math import sqrt

def loadMovieData(path = "./data/"):
    """
    Load movie data from u.data and u.item
    @param path: Data set path 
    """
    #Get movie's title
    movies = {}
    for line in open(path + '/u.item'):
        (movieId, movieTitle) = line.split('|')[0:2]
        movies[movieId] = movieTitle

    #Load Data
    movieData = {}
    userData = {}
    for line in open(path + '/u.data'):
        (userId, itemId, rating, timestamp)=line.split('\t')
        movieData.setdefault(movies[itemId], {})
        movieData[movies[itemId]][userId] = float(rating)
        userData.setdefault(userId, {})
        userData[userId][movies[movieId]] = float(rating)

    return (movieData, userData)

def euclidean(data, p1, p2):
    "Calculate Euclidean distance"
    distance = sum([pow(data[p1][item]-data[p2][item],2) 
                      for item in data[p1] if item in data[p2]])

    return 1.0 / (1 + distance)

def pearson(data, p1, p2):
    "Calculate Pearson correlation coefficient"    
    corrItems = [item for item in data[p1] if item in data[p2]]

    n = len(corrItems)
    if n == 0: 
        return 0;

    sumX = sum([data[p1][item] for item in corrItems])
    sumY = sum([data[p2][item] for item in corrItems])
    sumXY = sum([data[p1][item] * data[p2][item] for item in corrItems])
    sumXsq = sum([pow(data[p1][item], 2) for item in corrItems])
    sumYsq = sum([pow(data[p2][item],2) for item in corrItems])         

    if sqrt((sumXsq - pow(sumX, 2) / n) * (sumYsq - pow(sumY, 2) / n)) != 0:
        pearson = (sumXY - sumX * sumY / n) / sqrt((sumXsq - pow(sumX, 2) / n) * (sumYsq - pow(sumY, 2) / n))
    else:
        return 0

    return pearson

def getSimilarItems(movieData, n = 20, similarity=pearson):
    """
    Create a dictionary of items showing which other items they are most similar to.
    """

    results = {}
    for movie in movieData:
        #Get n items which most similar to movie
        matches = [(similarity(movieData, movie, otherMovie),otherMovie) 
                  for otherMovie in movieData if movie != otherMovie]
        matches.sort()
        matches.reverse()
        results[movie] = matches[0:n]

    return results

def getRecommendationsItems(userData, user, similarItems, n = 10):
    """
    Get recommendations items for user
    """ 
    userRatings = userData[user]
    itemScores = {}
    totalSim = {}

    # Loop over items rated by this user
    for (item, rating) in userRatings.items():
        # Loop over items similar to this one
        for (simValue, simItem) in similarItems[item]:
            # Ignore if this user has already rated this item
            if simItem in userRatings:
                continue
            # Weighted sum of rating times similarity
            itemScores.setdefault(simItem, 0)
            itemScores[simItem] += simValue * rating
            # Sum of all the similarities
            totalSim.setdefault(simItem, 0)
            totalSim[simItem] += simValue

    # Divide each total score by total weighting to get an average  
    rankings = [(score / totalSim[item], item) for (item, score) in itemScores.items() if totalSim[item] != 0]
    rankings.sort()
    rankings.reverse()

    return rankings[0:n]

if __name__ == "__main__":

    print 'Loading Data...'
    movieData, userData = loadMovieData("./movie/")
    print 'Get similarItems...'
    similarItems = getSimilarItems(movieData, 50, euclidean)
    print 'Calculate rankings...'
    rankings = getRecommendationsItems(userData, "87", similarItems)

    print rankings
```

