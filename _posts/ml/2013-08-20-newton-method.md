---
layout: post
title: "牛顿法"
categories:  [ml, newton, programming]
---

牛顿法在数值计算中可以用来求解方程式，也可以在优化问题中求极值，分别对应下面两个例子～

###牛顿迭代求解Sqrt问题

求解`x=Sqrt(n)`，令$$f(x)=x^2 -n$$，如下图所示:

![newton-sqrt](/image/newton-sqrt.jpg "newton-sqrt")

求得$$(x_i, f(x_i))$$点的切线方程为：$$f(x) = f(x_i) + f'(x_i)(x-x_i)$$，其中$$f'(x)=2x$$

由于我们要求的$$f(x)=x^2-n=0$$，所以，令切线方程$$f(x_{x+1})=0$$，得：

$$x_{i+1} = (x_i + n/x_i)/2$$

之后就可以通过上面迭代公式求解:

    public int sqrt(int x){
      double res = 1;
      double last = 0;
      
      while(res != last){
        last = res;
        res = (res + x/res)/2;
      }
      
      return (int)res;
    }

参考：[1](http://www.cnblogs.com/AnnieKim/archive/2013/04/18/3028607.html)


###牛顿法求解优化问题

牛顿法求解优化问题的思路和上面求解$$f(x)=0$$一致，唯一不同的是优化问题中都是求最小(大)值，需要转化一下,考虑到极值点梯度为0，所以牛顿法就是通过求解梯度为0来解决优化问题的

考虑无约束优化问题： $$ \min_{x \in R^n} f(x) $$

假设$$f(x)$$具有连续二阶偏导数，第k次迭代值为$$x^{(k)}$$，将$$f(x)$$在$$x^{(k)}$$处进行二阶泰勒展开：

$$f(x) = f(x^{(k)}) + g_k^T*(x-x^{(k)}) + \frac12 (x-x^{(k)})^T H(x^{(k)}) (x-x^{(k)})$$

其中，$$g_k^T \text{是} f(x) \text{在} x^{(k)}$$点的梯度向量；$$H(x^{(k)}) \text{是} f(x) \text{在} x^{(k)}$$点的海赛矩阵(Hesse matrix)

$$
H(x)=
\begin{bmatrix}
\frac{\partial^2 f}{\partial_i \partial_j}
\end{bmatrix}_{n*n}
$$

函数f(x)有极值的必要条件是在极值点处梯度向量为0,假如第k+1次的迭代值$$x_{i+1}$$是极值的话，有：$$\nabla_{x^{(k+1)}}f=0$$

对泰勒展开式求导得：$$\nabla f(x) = g_k + H_k(x-x^{(k)})$$，所以$$ g_k + H_k(x^{(k+1)}-x^{(k)}) = 0 $$，最后得迭代公式为：

$$ x^{(k+1)} = x^{(k)} - H_k^{-1} g_k $$

牛顿法就是根据上面的迭代公式求最优值的，一般求值曲线如下：

![newton_optimization_vs_grad_descent](/image/newton_optimization_vs_grad_descent.jpg "newton_optimization_vs_grad_descent")

上图对比了梯度下降法(绿线)和牛顿法(红线)求解最小化问题的过程。梯度下降使用梯度(一阶导数)寻找下降最快的方向，而牛顿法通过曲率(二阶导数)寻找下降最快的方向，因此牛顿法的路径更直接。

>从几何上说，牛顿法就是用一个二次曲面去拟合你当前所处位置的局部曲面，而梯度下降法是用一个平面去拟合当前的局部曲面，通常情况下，二次曲面的拟合会比平面更好，所以牛顿法选择的下降路径会更符合真实的最优下降路径。

###拟牛顿法
在牛顿法的迭代中，需要计算海赛矩阵的逆，计算复杂度很高，考虑使用一个n阶矩阵$$G_k=G(x^{(k)})$$**近似**代替$$H_k^{-1}$$。这就是逆牛顿法的思路
