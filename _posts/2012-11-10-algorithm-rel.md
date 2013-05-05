---
layout: post
title: "Algotithm Rel"
category: 
---

## 神奇的异或运算
1. 异或运算(Xor)是**位运算**的一种
2. 具有交换率,结合率：`a^b^b = a^(b^b)= a^0= a`.这些性质有两个显著应用：

    * 两数交换
      void swap(int a, int b){
        a=a^b; b=a^b; a=a^b;
      }

    * 找出单独的数 
      现在给你2n+1个正整数,其中有n对数和1个单独的数,这里规定一对数的意思是这两个数相等,然后让你设计一种算法,把这个单独的数给找出来,要求时间复杂度为O(n). 
      很简单,把2n+1个数全部进行异或操作,最后得到的数就是那个单独的数.(利用了交换率)  
      如果有两个单独的数怎么处理？**思路是转化为已知子问题***,[参考](http://blog.csdn.net/morewindows/article/details/8214003)

3. 异或的另外一个神奇性质是满足消去率,即`a^c = b^c` ==> `a==b`.这个特性可以解决[Nim游戏](http://baike.baidu.com/view/1101962.htm)问题：    
    像Nim游戏这种博弈问题,最重要的是寻找必败态.这个必败态的的意思就是,这样一种局面摆在面前的话先手必败.其严格定义如下: 

    1. 无法进行任何移动的局面是必败态；
    2. 可以移动到必败态的局面是非必败态；
    3. 在必败态做的所有操作的结果都是非必败态.  
  
    就是自己处在非必败态上总能移动到必败态把必败态留给对方,而对方处在必败态的话总是只能移动到非必败态,把非必败态留给自己,然后自己继续虐对方.  
    而对于Nim游戏,局面是必败态当且仅当所有堆硬币的数量都异或起来结果为0,即a1^a2^...^an=0,我们只要证明它满足上述必败态的三条性质即可.

    1. 第一个命题显然,最终局面只有一个,就是全0,异或仍然是0.
    2. 第二个命题,对于某个局面(a1,a2,...,an),若a1^a2^...^an!=0,一定存在某个合法的移动,将ai改变成ai’后满足a1^a2^...^ai’^...^an=0.不妨设a1^a2^...^an=k,则一定存在某个ai,它的二进制表示在k的最高位上是1（否则k的最高位那个1是怎么得到的）.这时ai^k<ai一定成立.则我们可以将ai改变成ai’=ai^k,此时a1^a2^...^ai’^...^an=a1^a2^...^an^k=0.
    3. 第三个命题,对于某个局面(a1,a2,...,an),若a1^a2^...^an=0,一定不存在某个合法的移动,将ai改变成ai’后满足a1^a2^...^ai’^...^an=0.因为异或运算满足消去率,由a1^a2^...^an=a1^a2^...^ai’^...^an可以得到ai=ai’.所以将ai改变成ai’不是一个合法的移动.证毕.


4. 参考: [Nim游戏的必胜策略和Xor运算的神奇应用](http://www.physixfan.com/archives/563)

## 找寻最大循环周期 
[HDU1021 Fibonacci Again](http://acm.hdu.edu.cn/showproblem.php?pid=1021)  
There are another kind of Fibonacci numbers: F(0) = 7, F(1) = 11, F(n) = F(n-1) + F(n-2) (n>=2). Judge if 3 divide evenly into F(n).

这道题目输出前20个结果就能总结出规律,但是可以从理论上证明这个结果序列是循环的,而且可以确定最大循环的周期,考虑到两点：
  * 每个数字由前两个决定,也就是说两个连续的数字重复后,就会出现循环（Fabonacci的重要规律）
  * 被3整除,使得每个结果只有0,1,2三种可能,连续两个结果的组合有`3*3=9`中组合,所以循环周期最大为9
这个貌似可以考虑计算理论中的有限状态机原理...

## 预测分支
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
