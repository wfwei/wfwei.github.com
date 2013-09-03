---
layout: post
title: "正则化和模型选择"
categories: [ml, regularization, model-selection]
---

这篇笔记主要记载机器学习中的**正则化和模型选择**方法，参考Andrew NG的公开课[笔记5]()

模型选择涉及一下几个方面：

###交叉验证(Cross Validation)

在训练(选择)模型的时候，如果只考虑最小化模型在训练集的经验误差，通常会导致模型较复杂，出现过拟合现象，在新数据上的泛化能力(在新数据上的预测能力)较低

为此，可以使用交叉验证的方法防止模型过拟合，基本思想就是将训练集中分离出部分数据做交叉验证数据集，之后，在训练集上训练模型，在交叉验证集上测试和选择模型，这样，由于用来测试的数据没有参加训练，可以更好的衡量模型的泛化能力


交叉验证具体有三种方式：

1. Hold-out Cross Validation(又称Simple CV): 随机从训练集中拿出部分(如30%)的数据做交叉验证
2. K-fold Cross Validation: 将训练集合分成K个子集，交叉训练验证K次，每次选择K-1份子集做训练，另外一份子集做交叉验证
3. Leave-One-Out Cross Validation: 当数据极度匮乏的时候，令2中的K为训练样本个数

###Feature Selection

当现有的Feature数量n远远大于样本数量m的时候，且现有feature中只有一小部分学习的目标相关，这时，训练的模型很容易过拟合，因此有必要设法降低feature的数量

给定n个feature，可能的feature子集有2ⁿ个，如果直接把这个feature selection的过程看作model selection的话，即在这2ⁿ个模型上做选择，代价是不可接受的，因此，一般都会用一些启发式的搜索算法做feature selection

➀ **Wrapper Feature Selection**

1. Forward Search 前向搜索

    ![feature-selection-forward-search](/image/feature-selection-forward-search.png)

2. Backward Search 反向搜索
        和前向搜索正好相反，其初始化feature集合为所有feature，不断删减没用的feature

Wrapper Feature Selection 效果都比较好，但是计算量比较大，需要调用大概O(n^2)次训练验证算法

➁ **Filter Feature Selection**

Filter Feature Selection大大降低了计算量

分别计算每个feature的S(i)值，用来描述“how informative each feature $$x_i$$ is about the class labels $$y$$”，“one possible choice of the score is the correlation between $$x_i$$ and $$y$$, as measured on the training data”，之后选取S(i)最大的k个feature

经常使用$$x_i$$ 和 $$y$$的互信息 $$MI(x_i, y)$$作为S(i)

$$
MI(x_i, y) = \sum_{x_i \in \{0,1\}} \sum_{y \in \{0,1\}} p(x_i, y)\log\frac{p(x_i, y)}{p(x_i)p(y)}
$$

上式中$$x_i, y$$都是binary value的

互信息可以表示为KL散度(Kullback-Leiber divergence):

$$ MI(x_i, y) = KL(p(x_i, y) \| p(x_i)p(y)) $$
  
其主要用来衡量概率分布$$p(x_i, y)$$和$$p(x_i)p(y)$$的差异，如果$$x_i$$和$$y$$是独立的，则有$$p(x_i, y) = p(x_i)p(y)$$，这样两个分布之间的KL散度为0，这和互信息含义是一致的，如果二者独立，即$$x_i$$对$$y$$没有任何信息量，所以$$MI(x_i,y)=0$$

