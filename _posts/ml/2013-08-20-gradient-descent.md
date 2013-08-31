---
layout: post
title: "梯度下降"
categories: [ml, gradient-descent]
---

梯度下降法，也称最速下降法，是求解无约束最优化问题的常用方法

假设$$f(x)$$在$$R^n$$上有连续偏导数，要求解的无约束优化问题为：

$$ \min_{x \in R^n} f(x) $$

梯度下降法是一种迭代算法，由于**负梯度**方向是函数下降最快的方向，所以在迭代每一步的时候，以负梯度方向不断更新$$x$$，这样会使$$f(x)$$在**当前位置**下降最快

当目标函数是凸函数的时候，梯度下降的解是全局最优解，但一般情况下，并不能保证；同时，梯度下降的收敛速度有不一定是最快的，可以参考下一篇[牛顿下降法](/posts/newton-method/)

### 简单证明

由于$$f(x)$$具有一阶连续偏导数，若第k次迭代值为$$x^{(k)}$$，将$$f(x)$$在$$x^{(k)}$$处一阶泰勒展开得：$$f(x) = f(x^{(k)}) + \nabla f(x^{(k)}) (x-x^{(k)})$$

带入$$x^{(k+1)}$$得： $$ f(x^{(k+1)}) = f(x^{(k)}) + \nabla f(x^{(k)}) (x^{(k+1)}-x^{(k)}) $$

梯度下降是一种迭代方法，可以假设迭代公式为:$$ x^{(k+1)} = x^{(k)} + \lambda_k p_k $$，其中$$\lambda_k (>0)$$是移动的步长，$$p_k$$是移动的方向

步长是用来调节移动速度的，可人工设定和调节，关键是确定移动方向$$p_k$$，方向错了，步长调节根本无效，算法就失效了

结合泰勒展开式和迭代公式，最有问题可以转化为：

$$
\begin{aligned}
\min f(x^{(k+1)}) 
&\sim  \min \nabla f(x^{(k)}) (x^{(k+1)}-x^{(k)}) \\
&\sim  \min \nabla f(x^{(k)}) (x^{(k)} + \lambda_k p_k -x^{(k)}) \\
&\sim  \min \lambda_k \nabla f(x^{(k)}) p_k  \\
\end{aligned}
$$

因为步长$$\lambda_k > 0$$，所以当$$p_k = -\nabla f(x^{(k)})$$的时候，优化问题可以得到最优解，因此最终的迭代公式为：

$$x^{(k+1)} = - \lambda_k \nabla f(x^{(k)})$$


### 方法分类

这里的笔记本来来自[回归分析](/posts/regression/)，所以一些符号表示不一致，这里重新说明一下：


代价方程和优化问题定义如下：

$$ J(\theta) = \frac{1}{2} \sum_{i=1}^{m} ( h_\theta(x^{(i)}) - y^{(i)}  )^2 $$

$$ min_\theta J(\theta) $$

梯度下降(Gradient Descent GD)算法

使$$\theta$$想梯度反方向变化，即: $$\theta_j := \theta_j - \alpha \frac{\partial J(\theta)}{\partial \theta_j} $$

其中 $$\alpha$$ 是学习速率，$$\frac{\partial J(\theta)}{\partial \theta_j} = (h_\theta(x) - y) x_j $$

根据迭代方式的不同，梯度下降又分为**(批量)梯度下降(Batch GD)和随机梯度下降(Stochastic GD)**

1 梯度下降：每次迭代都要遍历所有训练数据计算梯度

$$
\text{Repeat until convergence \{ }\\
~~~~~~~~~~~~~~~\theta_j := \theta_j - \alpha \sum_{i=1}^{m} (h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}\\
~~~~~~\text{\}}
$$

2 随机梯度下降：每次迭代只遍历单个训练数据

$$
\text{Repeat until convergence \{ }\\
~~~~~~~~~~~\text{for i=1 to m  \{ }\\
~~~~~~~~~~~~~~~\theta_j := \theta_j - \alpha (h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}\\
~~~~~~~~~~~\text{\}}\\
~~~~~~\text{\}}
$$

3 中和上面两种，每次使用k个训练数据更新参数，即是另一种方法：miniBatch GD


梯度下降和随机梯度下降的比较：

1. 计算代价，梯度下降每次迭代都要遍历所有训练数据，因此，大数据下，随机梯度下降较常用
2. 收敛速度，随机梯度下降算法快一些，但可能永远无法收敛
3. 局部最优，二者都可能

