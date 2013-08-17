---
layout: post
title: "分类问题"
categories: [ml, classification]
---

##分类的基本算法有

1. 非优化模型： 最近邻算法(kNN)，贝叶斯(Bayes)，决策树(DecisionTree)
2. 优化模型：感知机(Perceptron)，逻辑回归(LogisticRegression)，支持向量机(SVM)

##感知机

二类分类线性模型:

$$ f(x)=sign(w^Tx+b) \\$$
$$ sign(x)=\begin{cases}1, & \text{ if } x>=0 \\ -1, & \text{ if } x<0 \end{cases} $$

损失函数选择：

1. 首先想到误分类点个数，但这样的损失函数不是参数w和b的可导函数，不易优化
2. 另一个选择是误分类点到超平面的几何距离总和:
$$ -\frac{1}{\|w\|} \sum_{x_i\in M}y_i(w^Tx_i+b) $$  
其中M是误分类点集合(该函数是支持向量机的损失函数)
3. 感知机简化了2中的模型，去掉了$$ \frac{1}{\|w\|} $$，使用误分类点到超平面的函数距离之和，其形式为：
$$ L(w,b)=-\sum_{x_i\in M}y_i(w^Tx_i+b) $$

对于误分类点，$$y_i$$和$$ wx_i+b $$的符号刚好相反，所以前面加一个负号，使用梯度下降或牛顿法求最小值

感知机的对偶算法，思想就是用拉格朗日乘子线性组合来表示w，具体优势在SVM算法中才有体现

##逻辑回归

逻辑回归是在感知机模型上加了Logistic函数映射：

$$ Logistic(x)= \frac{1}{1+exp(x)} $$

Logistic函数值域为(0,1)，因此，类别用0,1表示，即$$y_i=0,1$$

由于Logistisc函数非线性，如果使用最小二乘去求解，计算复杂

__因此逻辑回归根据最大熵原理，使用极大似然做参数估计__:

似然函数为:   
$$ max ~ \prod_{i=1}^{N} [P(Y=1|x_i)]^{y_i}[P(Y=0|x_i)]^{1-y_i} $$

其中:   
$$ P(Y=1|x)=Logistic(w^Tx)  P(Y=0|x)=1-P(Y=1|x) $$

化简后得:   
$$ max ~ \sum_{i=1}^{N} [y_i(w^Tx_i)-log(1+exp(w^Tx_i))] $$

之后使用梯度下降或牛顿法求最优值

##支持向量机

感知机模型中使用的是误分类点到超平面的*函数距离*

在超平面$$w^Tx+b=0$$确定时，函数间隔$$-y_i(wTx_i+b)$$只能**相对地**表示点到超平面的远近

在选择超平面的时候，只有函数间隔是不够的，只要成倍的改变w,b，超平面不变，但函数距离就成倍变化

由于我们关注的是超平面，所以可以把函数间隔或w,b固定下来，一般选择固定函数间隔，容易优化

支持向量机是求解能够正确划分数据集并且几何间隔最大的超平面，推导过程：

最大间隔超平面

$$ 
\max_{w,b} ~ \gamma \\ 
s.t.       ~ y_i(\frac{\omega^T}{\|\omega\|}x_i+\frac{b}{\|\omega\|})>=\gamma, \text{i=1,2,...,N} 
$$

即，最大化超平面(w,b)关于数据集合的集合间隔$$\omega$$

考虑函数间隔和几何间隔的关系: $$ \gamma=\frac{\tilde{\gamma}}{\|\omega\|} $$

改写最优化问题为:
$$
\max_{w,b} ~ \frac{\tilde{\gamma}}{\|\omega\|} \\
s.t. ~ y_i(w^Tx_i+b)>=\tilde{\gamma} ~ \text{, i=1,2,...,N}
$$

由于函数间隔$$\tilde{\gamma}$$的值不影响最优化问题的解，因此可以取$$\tilde{\gamma}=1$$，改写最优化问题为:

$$
min_{w,b} ~ \|\omega\|^2 \\
s.t. ~ y_i(w^Tx_i+b)>=1 ~ \text{, i=1,2,...,N}
$$

##对偶问题的推导

首先构建拉格朗日函数，对每个不等式约束引进拉格朗日乘子$$\alpha_i >=0$$并放到目标函数中:

$$L(w,b,\alpha) = \frac{1}{2} \|w\|^2 - \sum_{i=1}^N \alpha_i(y_i(w^Tx_i+b)-1)$$

令： $$\theta(w) = \max_{\alpha_i\geq 0}L(w,b,\alpha)$$

容易验证，但有约束不满足的时候，$$\theta(w)=+\infty$$，比如当$$y_i(w^Tx_i+b) < 1$$时，只要令$$\alpha_i = +\infty$$即可

而当所有条件都满足时，$$\theta(w) = \frac{1}{2} \|w\|^2 $$，即我们最初的优化问题

因此，原始问题等价与$$\min \theta(w)$$，即：

$$\min_{w,b}\; \theta(w) = \min_{w,b}\; \max_{\alpha}\; L(w, b, \alpha) = p^* $$

这里，$$p^*$$表示该问题的最优解，也即原始问题的最优解

如果把极大，极小的位置转换一下，得：

$$\max_{\alpha}\;\min_{w,b}\;L(w, b, \alpha) = d^* $$

交换后的问题不再等价与原问题，称为原始问题的对偶问题，其最优值记为$$d^*$$

直观上，可以看出$$d^* \leq p^*$$，称为弱对偶，其实对于SVM，$$d^* = p^*$$，称为强对偶

如果原始问题是 Convex 的并且满足 Slater 条件的话，那么 strong duality 成立（充分条件）

Slater条件是指存在严格满足约束条件的点

还有KKT条件。。。

##对偶问题的计算

首先L关于w和b最小化，令其偏导数为0得：

$$
\begin{align} 
\frac{\partial L}{\partial w} = 0 &\Rightarrow w=\sum_{i=1}^N \alpha_i y_i x_i \\ 
\frac{\partial L}{\partial b} = 0 &\Rightarrow \sum_{i=1}^N \alpha_i y_i = 0 
\end{align}
$$

带回L得：

$$
\begin{align} 
L(w,b,\alpha) &= \frac{1}{2}\sum_{i,j=1}^n\alpha_i\alpha_jy_iy_jx_i^Tx_j-\sum_{i,j=1}^n\alpha_i\alpha_jy_iy_jx_i^Tx_j - b\sum_{i=1}^n\alpha_iy_i + \sum_{i=1}^n\alpha_i \\ 
&= \sum_{i=1}^n\alpha_i - \frac{1}{2}\sum_{i,j=1}^n\alpha_i\alpha_jy_iy_jx_i^Tx_j 
\end{align}
$$

此时，我们得到关于对偶变量$$\alpha$$的优化问题：

$$
\begin{align} 
\max_\alpha &\sum_{i=1}^n\alpha_i - \frac{1}{2}\sum_{i,j=1}^n\alpha_i\alpha_jy_iy_jx_i^Tx_j \\ 
s.t., &\alpha_i\geq 0, i=1,\ldots,n \\ 
&\sum_{i=1}^n\alpha_iy_i = 0 
\end{align}
$$

该问题可以使用高效的SMO算法求解

该问题求解后得到$$\alpha$$，可以根据前面求导得到的$$w=\sum_{i=1}^N \alpha_i y_i x_i$$关系计算w，之后选取一个支持向量($$\alpha_j \neq 0$$对应的向量)计算b，$$ b=y_j-\sum_{i=1}^Ny_i\alpha_i(x_i \cdot x_j) $$

将w带回原来的超平面方程得：

$$
\begin{align} 
f(x)&=\left(\sum_{i=1}^n\alpha_i y_i x_i\right)^Tx+b \\ 
    &= \sum_{i=1}^n\alpha_i y_i \langle x_i, x\rangle + b 
\end{align}
$$

这样，对于新点x的预测，只需要计算其与训练数据的内积即可(_这是使用kernel进行非线性扩展的前提_)
同时，'Support Vector'也在这里显现出来，因为，对于所有非'Support Vector'，对应的$$\alpha$$都为0
因此，对于新点的内积，只需要少量的'支持向量'，而不是所有的数据

##核kernel

对于线性不可分的数据，我们通常需要将原始数据映射到高维空间，使其线性可分

最简单的做法，我们可以设定一个将数据从映射到高维的函数$$\phi(x)$$，这样分类函数表示为：

$$ f(x) = \sum_{i=1}^n\alpha_i y_i \langle \phi(x_i), \phi(x)\rangle + b $$

其中，$$\alpha$$是通过求解如下对偶问题得到的：

$$
\begin{align} 
\max_\alpha &\sum_{i=1}^n\alpha_i - \frac{1}{2}\sum_{i,j=1}^n\alpha_i\alpha_jy_iy_j\langle \phi(x_i),\phi(x_j)\rangle \\ 
s.t., &\alpha_i\geq 0, i=1,\ldots,n \\ 
&\sum_{i=1}^n\alpha_iy_i = 0 
\end{align}
$$

这个方法说白了就是，拿到非线性数据，找到一个映射函数(不一定映射到高维空间，只要能使数据线性可分)，然后将数据通过映射函数映射到新空间，再做线性SVM

假设当前数据2个特征(a,b)，映射到三维空间的话，做多有如下多种维度:

* 0个a：b, bb, bbb
* 1个a: a, ba, bba
* 2个a：aa, aab
* 3个a: aaa

上面貌似不对...反正特征随着维度的增加，呈爆炸式增长

设两个变量$$x_1 = (\eta_1,\xi_1)^T$$和$$x_2 = (\eta_2,\xi_2)^T$$

设映射函数为：
$$\phi(x_1)=[\sqrt{2}\eta_1,\eta_1^2,\sqrt{2}\xi_1,\xi_1^2,\sqrt{2}\eta_1\xi_1, 1]^T$$

$$ \langle \phi(x_1),\phi(x_2)\rangle = 2\eta_1\eta_2 + \eta_1^2\eta_2^2 + 2\xi_1\xi_2 + \xi_1^2\xi_2^2 + 2\eta_1\eta_2\xi_1\xi_2 + 1  $$

同时:

$$
\begin{align}
\left(\langle x_1, x_2\rangle + 1\right)^2 
&= (\eta_1\eta_2 + \xi_1\xi_2 + 1)^2 \\
&= 2\eta_1\eta_2 + \eta_1^2\eta_2^2 + 2\xi_1\xi_2 + \xi_1^2\xi_2^2 + 2\eta_1\eta_2\xi_1\xi_2 + 1 
\end{align}
$$

我们发现：

$$\left(\langle x_1, x_2\rangle + 1\right)^2 = \langle \phi(x_1),\phi(x_2)\rangle$$

二者相等，不同在于：一个是映射到高维空间中，然后再根据内积的公式进行计算；而另一个则直接在原来的低维空间中进行计算，而不需要显式地写出映射后的结果。

我们把这里的计算两个向量在映射过后的空间中的内积的函数叫做核函数 (Kernel Function)，上面例子中的核函数为:

$$k(x_1,x_2)=\left(\langle x_1, x_2\rangle + 1\right)^2$$

核函数能简化映射空间中的__内积运算__——刚好“碰巧”的是，SVM里需要计算的地方，数据向量总是以内积的形式出现的。

因此SVM中的优化问题和决策平面改写为核函数的形式分别为：

决策面函数:

$$
f(x) = \sum_{i=1}^n\alpha_i y_i k(x_i, x) + b
$$

优化问题:

$$
\begin{align} 
\max_\alpha &\sum_{i=1}^n\alpha_i - \frac{1}{2}\sum_{i,j=1}^n\alpha_i\alpha_jy_iy_jk(x_1,x_2) \\ 
s.t., &0\leq \alpha_i\leq C, i=1,\ldots,n \\ 
&\sum_{i=1}^n\alpha_iy_i = 0 
\end{align}
$$

常见的核函数有：

1. 多项式核: $$\kappa(x_1,x_2) = \left(\langle x_1,x_2\rangle + R\right)^d$$
2. 高斯核: $$\kappa(x_1,x_2) = \exp\left(-\frac{\|x_1-x_2\|^2}{2\sigma^2}\right)$$
3. 线性核: $$\kappa(x_1,x_2) = \langle x_1,x_2\rangle$$

##异常点Outliers

如果数据线性不可分，可以通过尝试使用核函数将数据映射到高维空间，寻找可能的分隔超平面

但数据常常含有噪音，这些异常点会严重影响超平面的位置，有时，仅仅是因为异常点就导致找不到超平面，如下图中的黄色点：

![svm-with-noise](/image/svm-with-noise.png)

为了处理这种情况，SVM 允许数据点在一定程度上偏离一下超平面，改写优化问题为：

$$
\begin{align} 
\min & \frac{1}{2}\|w\|^2 + C\sum_{i=1}^n\xi_i \\ 
s.t., & y_i(w^Tx_i+b)\geq 1-\xi_i, i=1,\ldots,n \\ 
& \xi_i \geq 0, i=1,\ldots,n 
\end{align}
$$

其中，$$\xi_i \geq 0$$称为松弛变量(slack variable)，对应数据点$$x_i$$允许偏小的函数间隔的大小

如果我们允许$$\xi_i$$任意大的话，那任意超平面都满足要求了

所以需要在目标函数中加上一个正则项$$C\sum_{i=1}^n\xi_i$$，其中C是常数，用来权衡分割面间隔和数据点偏移量

这样，使用拉格朗日求解对偶问题过程如下：

$$L(w,b,\xi,\alpha,r)=\frac{1}{2}\|w\|^2 + C\sum_{i=1}^n\xi_i - \sum_{i=1}^n\alpha_i \left(y_i(w^Tx_i+b)-1+\xi_i\right) - \sum_{i=1}^n r_i\xi_i$$

其中$$\alpha,r ~ (\geq 0)$$是对偶变量，也即拉格朗日乘子

首先L关于$$w,b,r$$最小化，分别令其偏导数为0得：

$$
\begin{align} 
\frac{\partial L}{\partial w}=0 &\Rightarrow w=\sum_{i=1}^n \alpha_i y_i x_i \\ 
\frac{\partial L}{\partial b} = 0 &\Rightarrow \sum_{i=1}^n \alpha_i y_i = 0 \\ 
\frac{\partial L}{\partial \xi_i} = 0 &\Rightarrow C-\alpha_i-r_i=0, \quad i=1,\ldots,n 
\end{align}
$$

将w带回L化简得到和以前相同的目标函数：

$$\max_\alpha \sum_{i=1}^n\alpha_i - \frac{1}{2}\sum_{i,j=1}^n\alpha_i\alpha_jy_iy_j\langle x_i,x_j\rangle$$

这样，最终对偶问题为：

$$
\begin{align} 
\max_\alpha &\sum_{i=1}^n\alpha_i - \frac{1}{2}\sum_{i,j=1}^n\alpha_i\alpha_jy_iy_j\langle x_i,x_j\rangle \\ 
s.t., &0\leq \alpha_i\leq C, i=1,\ldots,n \\ 
&\sum_{i=1}^n\alpha_iy_i = 0 
\end{align}
$$

对比没有添加松弛变量前的对偶问题，发现只有对偶变量$$\alpha$$多了一个上限C

##数值优化--SMO算法

求解上面的对偶问题，SVM使用了高效的Sequential Minimal Optimization (SMO) 算法，其基本思路是：

如果所有变量的解都满足优化问题的KKT条件，那么这个优化问题的最优解得到了(KKT条件是优化问题的充要条件)；

否则，选取两个变量，固定其他的，针对这两个变量构建一个二次规划问题

该算法是对坐标下降算法( Coordinate Descend )的扩展

坐标下降法的原理是每次选取一个维度进行优化，比如求梯度，不断向梯度的反方向移动，即不断靠近最优值

考虑这里对偶变量的限制:$$ \sum_{i=1}^n\alpha_iy_i = 0 $$，如果每次选取一个维度$$\alpha_i$$进行优化，其他变量$$\alpha_j(j \neq i)$$视为变量，这样优化是没效果的，因为$$\alpha_i$$是确定的

所以，SMO每次每次选择两个坐标维度进行优化，(其中一个是违反KKT条件最严重的)，每个迭代步骤实际上是一个可以直接求解的一元二次函数极值问题，因此迭代高效

下面以选取α1和α2为变量，其余为常量为例，详细描述求解过程：

待解决的对偶问题描述为：

$$
\begin{align} 
\max_\alpha &\sum_{i=1}^n\alpha_i - \frac{1}{2}\sum_{i,j=1}^n\alpha_i\alpha_jy_iy_jk(x_1,x_2) \\ 
s.t., &0\leq \alpha_i\leq C, i=1,\ldots,n \\ 
&\sum_{i=1}^n\alpha_iy_i = 0 
\end{align}
$$

选取$$\alpha_1, \alpha_2$$为变量，固定其他$$\alpha_i~(i > 2)$$后得：

$$
\begin{align}
\max_{\alpha_1, \alpha_2} W(\alpha_1, \alpha_2) \\
W(\alpha_1, \alpha_2) = 
\alpha_1 + \alpha_2 - K_{11}\alpha_1^2-K_{22}\alpha_2^2-y_1y_2K_{12}\alpha_1\alpha_2 \\
- y_1\alpha_1\sum_{i=3}^N y_i\alpha_iK_{i1} - y_2\alpha_2\sum_{i=3}^N y_i\alpha_iK_{i2} \\
s.t. ~&~ \alpha_1y_1 + \alpha_2y_2 = -\sum_{i=3}^{N}y_i\alpha_i = \zeta \\
~&~ 0 \leq \alpha_1,\alpha_2 \leq C
\end{align}
$$

其中，$$K_{ij} = k(x_i,x_j)$$

因为$$\alpha_2 = \frac{1}{y_2} ( \zeta-\alpha_1y_1 ) \overset{y_2=-1,1}{==} y_2\left(\zeta-\alpha_1y_1\right)$$，将其带入目标函数可以消去α2，从而变成关于α1的一元函数，直接根据导数求极值

求导化简过程复杂，结果是：

$$
\alpha_1^{new} = \alpha_1^{old} + \frac{ y_1(E_2 - E_1) }{K_{11}+K_{22}-2K_{12}}
$$

其中 $$ E_i = f(x_i)-y_i = (\sum_{j=1}^n\alpha_j y_j k(x_j, x_i) + b) - y_i $$

![svm-smo-alpha](/image/svm-smo-alpha.png)

上图中红色线段为解范围

SMO使用一些启发式策略来选取最优的两个坐标维度，可以参见 John C. Platt 的那篇论文 Fast Training of Support Vector Machines Using Sequential Minimal Optimization

##TODO
1. 区分一下：分离超平面方程，分类决策函数，对偶问题
2. 线性支持向量机的最优解w*是唯一的，但b*却不唯一

##参考
1. 统计机器学习-李航博士
2. [支持向量机: Support Vector](http://blog.pluskid.orgkk/?p=682)
3. [支持向量机: Kernel](http://blog.pluskid.org/?p=685)
