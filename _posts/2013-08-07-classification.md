---
layout: post
title: "classification"
category: posts
---

分类的基本算法有

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

令： $$\theta(w) = \max_{\alpha_i\geq 0}\mathcal{L}(w,b,\alpha)$$

容易验证，但有约束不满足的时候，$$\theta(w)=+\infty$$，比如当$$y_i(w^Tx_i+b) < 1$$时，只要令$$\alpha_i = +\infty$$即可

而当所有条件都满足时，$$\theta(w) = \frac{1}{2} \|w\|^2 $$，即我们最初的优化问题

因此，原始问题等价与$$\min \theta(w)$$，即：

$$\min_{w,b}\; \theta(w) = \min_{w,b}\; \max_{\alpha}\; L(w, b, \alpha) = p^* $$

这里，$$p^*$$表示该问题的最优解，也即原始问题的最优解

如果把极大，极小的位置转换一下，得：

$$\max_{\alpha}\;\min_{w,b}\;L(w, b, \alpha) = d^* $$

交换后的问题不再等价与原问题，称为原始问题的对偶问题，其最优值记为$$d^*$$

直观上，可以看出$$d^* \leq p^*$$，需考虑再

这样，对偶问题的最优值$$d^*$$，为原始问题的最优值$$p^*$$的下界，同时，当问题满足KKT条件的时候，二者相等

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
\L(w,b,\alpha) &= \frac{1}{2}\sum_{i,j=1}^n\alpha_i\alpha_jy_iy_jx_i^Tx_j-\sum_{i,j=1}^n\alpha_i\alpha_jy_iy_jx_i^Tx_j - b\sum_{i=1}^n\alpha_iy_i + \sum_{i=1}^n\alpha_i \\ 
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

##SMO算法


##参考
1. 统计机器学习-李航博士
2. [支持向量机: Support Vector](http://blog.pluskid.org/?p=682)
