---
layout: post
title: "machine learning"
category: posts
---

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

启发：整合多弱分类器形成一个强大的分类器

原理：强学习算法（多项式时间，正确率高）；弱学习算法（正确率仅比随机猜测高）；二者等价！

