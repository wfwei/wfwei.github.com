---
layout: post
title: "监督学习(二)：分类问题"
categories: [ml, classification]
---

基本分类问题分类

## 生成 vs 判别

1. 生成模型，从数据中学习条件概率`P(X|Y)`及`P(Y)`，之后使用贝叶斯公式`p(Y|X)~P(Y)P(X|Y)`作为预测函数，即生成模型。典型的方法有高斯判别分析模型，贝叶斯以，隐马尔可夫，高斯混合模型、LDA、Restricted Boltzmann Machine等
2. 判别模型，从数据中直接学习判别函数`f(x)`或条件概率`P(Y|X)`，作为判别预测函数，即判别模型。典型方法包括kNN，感知机，决策树，线性回归、对数回归、线性判别分析、支持向量机、boosting、条件随机场、神经网络等
3. 二者关系：由生成模型可以得到判别模型，但由判别模型得不到生成模型

更多详见[这里](http://blog.sciencenet.cn/home.php?mod=space&uid=248173&do=blog&id=227964)

## 优化 vs 非优化

1. 非优化模型： 最近邻算法(kNN)，贝叶斯(Bayes)，决策树(DecisionTree)
2. 优化模型：感知机(Perceptron)，逻辑回归(LogisticRegression)，支持向量机(SVM)
