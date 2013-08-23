---
layout: post
title: "监督学习(一)：回归问题"
categories: [ml, supervised learning, regressiong]
---

本篇笔记主要记载监督学习基本概念，及回归问题的概念和推导

###监督学习

监督学习：通过学习带标签的训练数据，学习如何预测未知数据的标签问题

对于训练数据集 train-set: $$ {(x^{(i)}, y^{(i)}) , i=1,...,m} $$

学习得到一个函数h，给定x，能较好地c预测y，即：$$y=h(x)$$，

过程如下图所示：

![supervised-learing-process](/image/supervised-learing-process.png)

当预测目标变量(即y)是连续变量时，该问题称为回归问题；当目标变量取有限个离散值时，该问题称为分类问题

先引出一个具体问题：房价预测

训练集包括部分已知房价的房子的两个feature：面积和卧室数量；学习目标函数h能通过房子的面积和卧室数量，预测房价。

![house-price-problem](/image/house-price-problem.png)

约定符号如下：

* 训练数据个数: m
* feature 个数: n
* $$ x_j^{(i)} $$表示：第i条训练数据的第j个属性feature


###线性回归

为了学习预测函数，首先应该确定如何表示h，线性回归做了一个最简单的假设：y是关于x的线性函数

$$ h_\theta (x)  = \theta_0 + \theta_1 x_1 + \theta_2 x_2 $$

其中，$$\theta$$是参数，也称权重。为了简化表示，令$$x_0 = 1$$，则有：

$$ h_\theta = \sum_{i=0}^{n} \theta_i x_i = \theta^T x $$

现在我们需要通过训练集学习参数$$\theta$$，一个简单合理的方法是使h(x)在训练集上接近y，以此，我们定义代价方程(Cost Function)为：

$$ J(\theta) = \frac{1}{2} \sum_{i=1}^{m} ( h_\theta(x^{(i)}) - y^{(i)}  )^2 $$

我们的目标就是求解优化问题：$$ min_\theta J(\theta) $$


#### 1. LMS(Least Mean Square 最小均方)算法

搜索求解上面的最小化问题，开始初始化$$\theta$$(比如$$\frac{1}{n}$$)，不断改变$$\theta$$的值，使$$J(\theta)$$变小

具体算法包括：梯度下降和牛顿法

梯度下降(Gradient Descent GD)算法

使$$\theta$$想梯度反方向变化，即: $$\theta_j := \theta_j - \alpha \frac{\partial J(\theta)}{\partial \theta_j} $$

其中 $$\alpha$$ 是学习速率，$$\frac{\partial J(\theta)}{\partial \theta_j} = (h_\theta(x) - y) x_j $$

根据迭代方式的不同，梯度下降又分为梯度下降(Batch GD)和随机梯度下降(Stochastic GD)

1. 梯度下降：每次迭代都要遍历所有训练数据计算梯度


$$
\text{Repeat until convergence \{ }\\
~~~~~~~~~~~~~~~\theta_j := \theta_j - \alpha \sum_{i=1}^{m} (h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}\\
~~~~~~\text{\}}
$$

2. 随机梯度下降：每次迭代只遍历单个训练数据

$$
\text{Repeat until convergence \{ }\\
~~~~~~~~~~~\text{for i=1 to m  \{ }\\
~~~~~~~~~~~~~~~\theta_j := \theta_j - \alpha (h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}\\
~~~~~~~~~~~\text{\}}\\
~~~~~~\text{\}}
$$

3. 中和上面两种，每次使用k个训练数据更新参数，即是另一种方法：miniBatch GD

梯度下降和随机梯度下降的比较：

1. 计算代价，梯度下降每次迭代都要遍历所有训练数据，因此，大数据下，随机梯度下降较常用
2. 收敛速度，随机梯度下降算法快一些，但可能永远无法收敛
3. 局部最优，二者都可能

####2. The Normal Equations 等式求解

该方法不再像最小均方法一样迭代求解，而是直接令导数为0求极值，即求解$ \frac{\partial J(\theta)}{\partial \theta} = 0 $

向量表示J为：

$$
\begin{aligned}
J(\theta) &= \frac12 \sum_{i=1}^{m} ( h_\theta(x^{(i)}) - y^{(i)}  )^2 \\
&= \frac12 (X\theta-\vec{y})^T(X\theta-\vec{y})
\end{aligned}
$$

计算梯度为：

$$
\begin{aligned}
\nabla_\theta J 
&= \nabla_\theta(\frac12 (X\theta-\vec{y})^T(X\theta-\vec{y})) \\
&= X^TX\theta-X^T \vec{y}
\end{aligned}
$$

令梯度为0得：

$$
\begin{aligned}
X^TX\theta &= X^T \vec{y} \\
\theta &= (X^TX)^{-1}X^T\vec{y}
\end{aligned}
$$

可以直接通过上式求解最优解，但存在几个问题：

1. 公式中存在矩阵求逆操作，时间复杂度很高，不适合大数据下求解
2. 求逆要求矩阵非奇异，尤其对于稀疏数据来说，需要注意这点

### 概率角度解释最小平方代价函数J的合理性

假设对于每个目标变量y和自变量x，存在如下关系：

$$ y^{(i)} = \theta x^{(i)} +  \epsilon^{(i)}$$ 

其中$$\epsilon$$是误差，我们可以进一步假设$$\epsilon$$独立同分布于高斯分布:

$$\epsilon \sim N(0, \sigma^2)$$

则有：

$$
p(\epsilon^{(i)})=\frac{1}{\sqrt{2\pi} \sigma} exp\{ - \frac{(\epsilon^{(i)})^2}{2\sigma^2} \}
$$

则有:

$$
p(y^{(i)}|x{(i)};\epsilon^{(i)})=\frac{1}{\sqrt{2\pi} \sigma} exp\{ - \frac{( y^{(i)}-\theta^Tx^{(i)} )^2}{2\sigma^2} \}
$$

其中，$$p(y^{(i)}|x{(i)};\epsilon^{(i)})$$表示$$y^{(i)}$$关于$$x^{(i)}$$的条件概率，$$\theta$$是参数，而不是变量！

在训练数据集上使用极大似然估计得：

$$
\max L(\theta)\\
\begin{aligned}
L(\theta) &= \prod_{i=1}^m p(y^{(i)}|x{(i)};\epsilon^{(i)})\\
&= \prod_{i=1}^m \frac{1}{\sqrt{2\pi} \sigma} exp\{ - \frac{( y^{(i)}-\theta^Tx^{(i)} )^2}{2\sigma^2}\}
\end{aligned}
$$

由于包含指数连乘，所以使用对数似然函数：

$$
\begin{aligned}
\mathcal{L}(\theta) &= \log L(\theta)\\
&= \sum_{i=1}^m \log p(y^{(i)}|x{(i)};\epsilon^{(i)})\\
&= \sum_{i=1}^m \log \left\{ \frac{1}{\sqrt{2\pi} \sigma} exp\{ - \frac{( y^{(i)}-\theta^Tx^{(i)} )^2}{2\sigma^2} \} \right\}\\
&=m \log \frac{1}{\sqrt{2\pi} \sigma} - \frac{1}{2\sigma^2} \sum_{i=1}^m ( y^{(i)}-\theta^Tx^{(i)} )^2
\end{aligned}
$$

这样，$$\max \mathcal{L(\theta)}$$等价于：

$$ \min_\theta \frac{1}{2} \sum_{i=1}^m ( y^{(i)}-\theta^Tx^{(i)} )^2 $$

这也是最小二乘的代价方程！

注意，概率模型中做了几个假设，但这些假设并不是论证最小二乘代价方程正确性所必须的，同时，论证过程和假设的高斯分布的方差无关！

## 局部加权线性回归(Locally Weighted Linear Regression)

![locally-weighted-regression](/image/locally-weighted-regression.png "locally-weighted-regression")

上图中的点明显不是线性函数生成的，假如我们用线性回归去模拟，如红线所示，并不能完全和数据吻合，有很多点都落在了置信区间之外(红线两边的灰色地带)，导致预测准的下降

假如我们对x=2.5处的点进行预测，如果只是用区间[2,3]之间的数据点，效果如图中蓝色线，效果非常好，这就是**局部回归**的思想

在局部回归中，对某点进行预测仅仅选择其临近区域的样本，而其他样本则被浪费了，不免可惜，而且对该区域作回归时在临近区域内的样本对预测结果的影响是一样大的，一个更合理的想法是一个样本离某点越近，对该点的影响越大，这就引出了局部加权回归。
具体的做法是，在损失函数中加入权数，离目标点越近，权数越大：

$$
L(\theta) = \sum_{i=1}^{m} w^{(i)}( y^{(i)}-\theta^Tx^{(i)} )^2 \qquad  \textrm{其中}
w^{(i)} = e^{ - \frac {(x^(i)-x)^2} {2\alpha}  }
$$

其中，$$\alpha$$是波长参数(bandwidth parameter)，控制权数随距离下降的速度

局部回归有点如上所示，缺点：
1. 每次对一个点的预测都需要整个数据集的参与，样本量大且需要多点预测时效率低。提高效率的方法参考Andrew More的 KD Tree
2. 不可外推，对样本所包含的区域外的点进行预测时效果不好，事实上这也是一般线性回归的弱点


##参考
1. [Andrew NG 讲义笔记](http://cs229.stanford.edu/notes/cs229-notes1.pdf)
2. [机器学习 03-logisitc 回归](http://rstudio-pubs-static.s3.amazonaws.com/4810_06e3d8fd26ed40eb8c31aff35eae81ae.html)


