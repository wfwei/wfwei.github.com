---
layout: post
title: "machine learning"
category: posts
---
## 聚类
* 华盖聚类(Canopy)
  不需指定聚类个数，设置两个相似度阈值(T1<T2)，开始将所有点放到list中，循环该list，计算每个点和当前聚类的距离，小于T1则放到该聚类中，并从list中删除该点，如果距离小于T2大于T1,则该点暂时加入该类，但不从list中删除，如果距离大于T2,则开辟一个新的类。

* KMeans聚类
* KCentroid聚类
* 

##

## 推荐分类
* 基于人口统计学，如年龄，性别，地理位置  
  没有冷启动问题,且使用与不同物品(领域独立),但是无法给出对用于细节要求高的领域,如图书,音乐电影的推荐,同时,用户的信息不是很好获取
* 基于内容的推荐  
  基于推荐物品的‘元数据’，计算相关性，之后根据用户的喜好记录，推荐相似物品。比如对电影进行推荐，电影的类型,年代，导演演员等可以作为元数据，如果用户喜欢‘爱情，浪漫’的电影，可以将其他‘爱情浪漫’电影推荐。可以很好的建模用户喜好，提高推荐精度。但是，需要对物品进行分析建模，且推荐质量充分依赖于物品建模的方式和水平，且存在新用户的冷启动问题。
* 基于协同过滤的推荐  
基于用户对物品的偏好信息，计算用户的相似度，之后根据用户的历史喜好进行推荐。分为基于用户的协同过滤，基于项目的协同过滤以及基于模型的系统过滤
  * 基于用户的协同过滤,根据用户对物品的偏好，计算相似用户，根据相似用户的喜好向目标用户推荐,注意区分基于用户统计信息的推荐，二者都是发现相似用户，根据相似用户的喜好进行推荐，不同的是如何计算相似用户，基于人口统计学的机制只考虑用户本身的特征，而基于用户的协同过滤机制可是在用户的历史偏好的数据上计算用户的相似度.基于用户的协同过滤的基本假设是喜欢相同物品的用户有着类似的偏好
  * 基于项目的协同过滤,和基于用户的类似，根据用户对物品的偏好，计算物品的相似度，找到用户喜欢物品的相似物品进行推荐,基于项目的协同过滤推荐和基于内容的推荐其实都是基于物品相似度预测推荐，只是相似度计算的方法不一样，前者是从用户历史的偏好推断，而后者是基于物品本身的属性特征信息。
  * 基于模型的协同过滤,基于样本的用户喜好信息，训练一个推荐模型，然后根据实时的用户喜好的信息进行预测，计算推荐。不需要对用户和物品进行严格建模，方法的核心是历史数据挖掘，所以对新物品和新用户都有冷启动问题，同时推荐准度依赖于历史数据的多少和准确性，建模之后很难根据用户喜好改变而改变 ，不够灵活。
* 混合推荐机制  
  * 加权混合,用线性公式（linear formula）将几种不同的推荐按照一定权重组合起来
  * 切换混合,对于不同的情况（数据量，系统运行状况，用户和物品的数目等），推荐策略不同
  * 分区混合,采用多种推荐机制，并将不同的推荐结果分不同的区显示给用户
  * 分层混合,采用多种推荐机制，并将一个推荐机制的结果作为另一个的输入，从而综合各个推荐机制的优缺点，得到更加准确的推荐

## SVD奇异值矩阵分解

* 抽取重要特征，应用：feature reduction（降维），数据压缩，潜在语义（Latent Semetic）
* 特征值分解和奇异值分解目的是一样的，都是提取一个矩阵的重要特征
* 特征值分解只能用于方阵，而奇异值分解可以用于任何矩阵，奇异值的计算是利用特征之
    * 特征值，特征向量 `A*v = lambda*v` 向量v是矩阵A的特征向量，lambda是特征值
    * 分解：`A=Q*sigma*Q’` Q是矩阵A特征向量组成的矩阵，Q’是Q的逆矩阵，sigma是由矩阵A的特征值组成的对角阵
    * 加入A不是方阵，计算A*A’(A’是A的转置)的特征值lambda和特征向量v，则A的奇异值是lambda^0.5，奇异向量是A*v/lambda^0.5

##n-gram->language modeling->Markov property->Markov chain->Brownian motion
* n-gram:
在计算语言学和概率论领域，n-gram是从一个给定的文字序列中提取出的ｎ个连续文字。
    
    An n-gram of size 1 is referred to as a "unigram"; 
    size 2 is a "bigram" (or, less commonly, a "digram"); 
    size 3 is a "trigram". Larger sizes are sometimes referred to by the value of n, e.g., "four-gram", "five-gram", and so on.

An n-gram model is a type of probabilistic language model for predicting the next item. More concisely, an n-gram model predicts `xi` based on `xi-(n-1),...,xi-1`. In probability terms, this is P(xi|xi-(n-1),..,xi-1). 

When used for language modeling, independence assumptions are made so that each word depends only on the last n-1 words. This assumption is important because it massively simplifies the problem of learning the language model from data. 

* language modeling
A statistical language model assigns a probability to a sequence of m words `P(w1,...,wm)` by means of a probability distribution.In speech recognition and in data compression, such a model tries to capture the properties of a language, and to predict the next word in a speech sequence.

* Markov property
the conditional probability distribution of future states of the process depends only upon the present state, not on the sequence of events that preceded it. A process with this property is called a Markov process.  

* Markov chain
A system with discrete-time processes with the Markov property is known as a Markov chain.

* Brownian motion
布朗运动(Brownian motion) 过程是一种正态分布的独立增量连续随机过程。1827年英国植物学罗伯特·布朗利用一般的显微镜观察悬浮于水中由花粉所迸裂出之**微粒**时，发现微粒会呈现不规则状的运动，因而称它布朗运动。

##Boosting
* 启发：整合多弱分类器形成一个强大的分类器  
* 原理：强学习算法（多项式时间，正确率高）；弱学习算法（正确率仅比随机猜测高）；二者等价！

