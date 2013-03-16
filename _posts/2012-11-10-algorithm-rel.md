---
layout: post
title: "Algotithm Rel"
category: 
---

## 挖掘规律

1. HDU1021 [Fibonacci Again](http://acm.hdu.edu.cn/showproblem.php?pid=1021)

There are another kind of Fibonacci numbers: F(0) = 7, F(1) = 11, F(n) = F(n-1) + F(n-2) (n>=2). Judge if 3 divide evenly into F(n).

这道题目输出前20个结果就能总结出规律，但是可以从理论上证明这个结果序列是循环的，而且可以确定最大循环的周期，考虑到两点：
* 每个数字由前两个决定，也就是说两个连续的数字重复后，就会出现循环（Fabonacci的重要规律）
* 被3整除，使得每个结果只有0,1,2三种可能，连续两个结果的组合有`3*3=9`中组合，所以循环周期最大为9
这个貌似可以考虑计算理论中的有限状态机原理。。。

## Branch Prediction

Consider an if-statement: At the processor level, it is a *branch instruction*

    for (int c = 0; c < arraySize; ++c)
    {
        if (data[c] >= 128)
            sum += data[c];
    }

[full-ver](http://stackoverflow.com/questions/11227809/why-is-processing-a-sorted-array-faster-than-an-unsorted-array)




## Basic

1. input 
  * while(scanf("%s%s", &str1, &str2)!=EOF)...
  * char buf[20]; gets(buf)
  * char a = getchar()

2. output
  * printf("%d\n",res)
  * printf non-cached output

3. misc...
  * __int64--long long
  * sprintf(str, "%d", num)
  * int main()...
  * 
