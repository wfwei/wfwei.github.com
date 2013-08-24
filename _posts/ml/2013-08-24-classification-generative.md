---
layout: post
title: "监督学习(二)：生成分类模型"
categories: [ml, classification]
---

这篇笔记主要记载**GDA和朴素贝叶斯**的概念和推导

###高斯判别分析(Gaussian Discriminant Analysis GDA)

这个模型做了两个假设：

1. `P(X|Y)`满足高斯分布，详见[这里](/posts/ml-math/)
2. `P(Y)` 满足伯努利分布，相见[这里](http://zh.wikipedia.org/wiki/%E4%BC%AF%E5%8A%AA%E5%88%A9%E5%88%86%E5%B8%83)

GDA模型可表示为：

$$
\begin{aligned}
y       &\sim Bernoulli(\phi) \\
x|(y=0) &\sim \mathcal{N}(\vec{\mu_0},\Sigma)\\
x|(y=1) &\sim \mathcal{N}(\vec{\mu_1},\Sigma)
\end{aligned}
$$

模型中的参数包括$$\phi,\mu_0, \mu_1, \Sigma$$，GDA一般使用同一个协方差矩阵$$\Sigma$$分布：$$\mathcal{N}(\vec{\mu},\Sigma)$$

模型的概率密度表示为：

![gda-gradients](/image/gda-gradients.png "高斯判别分析的密度函数")

对数最大似然估计为：

![gda-max-likelihood](/image/gda-max-likelihood.png "高斯判别分析的对数极大似然估计")

根据导数为零做参数估计得：

![gda-parameters](/image/gda-parameters.png "高斯判别分析的参数估计")

>Question:感觉这个参数估计需要求导计算么？利用几何意义直接在训练集上统计不就有了么？

最后的结果为：

![gda-result](/image/gds-result.png)

由于假设中两个高斯分布的协方差相同，只是位置不同，所以可以用直线做类别分割

高斯判别分析(GDA)和逻辑回归的关系

使用GDA的密度函数计算Y关于X的概率函数`P(Y=1|X)`得：

$$P(Y=1 \mid X; \phi, \mu_0, \mu_1, \Sigma) \sim \frac{1}{1 + exp( - \theta^T x \} $$

和逻辑回归判别函数的形式是一致的

也就是说如果`p(x|y)`符合多元高斯分布，那么`p(y|x)`符合logistic回归模型。反之，不成立。为什么反过来不成立呢？因为**GDA有着更强的假设条件和约束**。

如果认定训练数据满足多元高斯分布，那么GDA能够在训练集上是最好的模型。然而，我们往往事先不知道训练数据满足什么样的分布，不能做很强的假设。Logistic回归的条件假设要弱于GDA，因此更多的时候采用logistic回归的方法。


###朴素贝叶斯(Naive Bayes)

在GDA中，我们要求特征向量x是连续实数向量。如果x是离散值的话，可以考虑采用朴素贝叶斯的分类方法。

可以参考以前的[笔记](/posts/bayesian/)

TODO 继续整理 

###参考：

* [公开课课件](http://cs229.stanford.edu/notes/cs229-notes2.pdf) 
* [课件翻译](http://www.cnblogs.com/jerrylead/archive/2011/03/05/1971903.html)
