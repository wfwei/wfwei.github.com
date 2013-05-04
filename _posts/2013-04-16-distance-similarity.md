---
layout: post
title: "自然语言处理相关"
category: posts
---
##距离
1. 范数和明可夫斯基距离
    * p范数：p-norm = sum(|Xi-Yi|^p for i=1 to n)^(1/p)
    * 明可夫斯基距离即是p范数
    * p=1,曼哈顿距离
    * p=2，欧氏距离
    * p-->无穷，切比雪夫距离
2. 曼哈顿距离（来源于城市区块距离）
    * dist(X, Y) = sum(|Xi-Yi| for i=1 to n)
3. 切比雪夫距离
    * dist(X, Y) = max(|Xi-Yi| for i=1 to n)
4. 欧氏距离
    * dist(X, Y) = sum(|Xi-Yi|^2 for i=1 to n)^0.5

##相似性
1. 余弦相似度
    * sim(X, Y) = cos(theta) = X*Y/(norm(X)*norm(Y))
    * 修正余弦就是另X=X-avg(X), Y=Y-avg(Y),之后再求余弦
2. Jaccard距离
    * Jaccard(X, Y) = (X交Y)/(X并Y)
3. 皮尔森相关系数
    * 衡量_线性相关_程度
    * Pxy = cov(X, Y)/(cov(X,X)^0.5*cov(X,X)^0.5)
    * cov(X, Y) = sum((Xi-avg(X))*(Yi-avg(Y)) for i=1 to n)/n

