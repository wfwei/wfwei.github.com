---
layout: post
title: "分解的思想求解Trapwater问题"
categories: [programming, algorithm, leetcode]
---

[LeetCode](http://leetcode.com/onlinejudge#question_13)解题报告，感觉这个题目的思路很好，可以举一反三，所以就记录一下～

###问题

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

For example, 
Given [0,1,0,2,1,0,1,3,2,1,2,1], return 6.

![rainwatertrap](/image/rainwatertrap.png)

###分析

题目要求上图中容器能够储藏多少水，如果直接求水的面积，其实是挺麻烦的，比如开始我的一个思路是递归求解，每次从第i个bar出发，找到地一个比i高的j，之后求i到j之间的储水量，之后另i=j，递归该过程，直到找到最高的i，图中对应i=7，如果i不是最后一个，则需要从最后到i计算中间的储水量，方法和前面一样，这样虽然能解决问题，代码写起来就略麻烦了～～

其实可以换个思路，不要直接求储水量，用注满水后的总面积减去柱子的总面积，计算储水量

* 柱子总面积很容易求，直接遍历所有柱子累加即可
* 注满水后的总面积，也很简单，思路就是分层计算，对第i层，找到两头的柱子，就得到该层的面积，当然，coding的思路是用两个指针分别指向前后两根柱子，并不断靠近，类似思路如[leetcode#11](http://leetcode.com/onlinejudge#question_11)

###源码

[mycode](https://gist.github.com/wfwei/6255698)
