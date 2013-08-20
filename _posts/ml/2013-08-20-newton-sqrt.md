---
layout: post
title: "Newton法求解"
categories:  [ml, newton, programming]
---

牛顿法在数值计算中可以用来求解方程式，也可以在优化问题中求极值，分别对应下面两个例子～

###牛顿迭代求解Sqrt

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

![newton_optimization_vs_grad_descent](/image/newton_optimization_vs_grad_descent.jpg "newton_optimization_vs_grad_descent")

上图对比了梯度下降法(绿线)和牛顿法(红线)求解最小化问题的过程。梯度下降使用梯度(一阶导数)寻找下降最快的方向，而牛顿法通过曲率(二阶导数)寻找下降最快的方向，因此牛顿法的路径更直接。

>从几何上说，牛顿法就是用一个二次曲面去拟合你当前所处位置的局部曲面，而梯度下降法是用一个平面去拟合当前的局部曲面，通常情况下，二次曲面的拟合会比平面更好，所以牛顿法选择的下降路径会更符合真实的最优下降路径。


