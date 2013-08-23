---
layout: post
title: "几何运算(Geometric Operation)"
categories: [ml, geometric]
---

常用的几何运算

几个共用变量：

$$A_{m*n} = 
\begin{pmatrix}
A_{11} & A_{12} & \cdots & A_{1n} \\
A_{21} & A_{22} & \cdots & A_{2n} \\
\vdots& \vdots & \ddots & \vdots \\
A_{m1} & A_{12} & \cdots & A_{mn}    
\end{pmatrix}$$

$$function~f:~\Re^{m*n} \mapsto \Re$$

###1. 矩阵秩

假设m=n，A的秩为：$$tr A = \sum_{i=1}^{n} A_{ii} $$

* 如果A是m×n，B是n×m的矩阵，则有: $$tr AB = tr BA$$
* 方阵A：$$tr A = tr A^T $$
* 对于方阵A,B: $$tr A+B = tr A + tr B $$
* 乘上alpha系数：$$tr \alpha A = \alpha tr A$$


###2. 矩阵梯度

$$
\frac{\partial f(A)}{\partial A_{m*n}} = 
\begin{pmatrix}
\frac{\partial f}{\partial A_{11}} & \frac{\partial f}{\partial A_{12}} & \cdots & \frac{\partial f}{\partial A_{1n}} \\
\frac{\partial f}{\partial A_{21}} & \frac{\partial f}{\partial A_{22}} & \cdots & \frac{\partial f}{\partial A_{2n}} \\
\vdots& \vdots & \ddots & \vdots \\
\frac{\partial f}{\partial A_{m1}} & \frac{\partial f}{\partial A_{12}} & \cdots & \frac{\partial f}{\partial A_{mn}}  
\end{pmatrix}
$$

例子：
A是2×2的矩阵，$$f(A)=\frac32 A_{11} + 5A_{12}^2 + A_{21}A_{22}$$，则有：

$$
\frac{\partial f(A)}{\partial A} = 
\begin{pmatrix}
\frac32 & 10 A_{12} \\
 A_{22} & A_{21}
\end{pmatrix}
$$

常用等式：

$$\nabla_{A^T} f(A) = (\nabla_A f(A))^T$$

$$\nabla_A \| A \| = \| A \| (A^{-1})^T$$

$$\nabla_A tr AB = B^T$$

$$\nabla_A tr ABA^TC = CAB+C^TAB^T$$


