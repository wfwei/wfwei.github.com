---
layout: post
title: "First Missing Positive"
categories: [algorithm, leetcode]
---

[LeetCode](http://leetcode.com/onlinejudge#question_41) 解题报告，这个题目开始弄了一个bigmap过了，后来发现更好解法，值得反思

###问题
Given an unsorted integer array, find the first missing positive integer.

For example,
  Given [1,2,0] return 3,
  and [3,4,-1,1] return 2.

Your algorithm should run in O(n) time and **uses constant space**.

###分析

题目要求时间复杂度O(n)，所以基于比较的排序算法都不用考虑了，但这个题目不排序基本上搞不定，所以只有使用‘计数排序’的思想了～

最直接的，打表法，只需要考虑positive integer，所以表长`2^31`，如果使用BitMap，只需要大概256M，也算是‘constant space’了==! 使用java，代码如下:

    public int firstMissingPositive(int[] A) {
      int MAX = Integer.MAX_VALUE >> 3 + 1;
      java.util.BitSet s = new java.util.BitSet(MAX);
      for (int a : A) {
        if (a > 0 && a <= MAX)
          s.set(a);
      }
      return s.nextClearBit(1);
    }

后来无意中看到一个[博客](http://www.cnblogs.com/AnnieKim/archive/2013/04/21/3034631.html)，其想法给我很大启示～～

核心思想还是‘计数排序’，只不过利用了该题目的一个隐含条件：**最小的缺失正整数肯定小于等于数组长度！**

这个限制很容易证明（比如用反证法），但我开始还是没观察到==！

利用这个这个限制，打表的时候就不需要考虑所有的正整数了，只需要大于数组长度即可，修改代码如下：

    public int firstMissingPositive(int[] A) {
      int MAX = Integer.MAX_VALUE;
      if (MAX > A.length)
        MAX = A.length;
      MAX = MAX>>3 + 1;
      java.util.BitSet s = new java.util.BitSet(MAX);
      for (int a : A) {
        if (a > 0 && a <= MAX)
          s.set(a);
      }
      return s.nextClearBit(1);
    }

这个代码对数组较短时做了优化，但是如果数组很长呢？仍要申请`A.length/8`的额外空间

为了切实把空间复杂度降低到O(1)，只能在原地计数排序了，原地排序就只能通过交换每个数字的位置实现～

因为只考虑正整数，所以映射关系为`A[i]<-i+1 , 0<=i<A.length` 

代码TODO
