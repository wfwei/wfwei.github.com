---
layout: post
title: 概率题目
Author: wfwei - cf.wfwei@gmail.com
Last modified: 2013-09-25 10:43:12
categories:[algorithm, probability]
---

> 一个木桶装有Ｍ个白球，小明每次从中取出一个球并涂成红色，并放回桶中：小明将所有白球涂成红色的期望时间是多少？

思路要稍微灵活一点～

定义将一个白球涂成红球这个事件为Ａ，那么题目就是要求事件Ａ发生Ｍ次的期望时间

事件Ａ发生k次后，桶中还有M-k个白球，这样，第k+1次发生的概率为`(M-k)/M`，期望时间就是`M/(M-k)`

则事件A发生M次的总期望时间就是 $$\sum_{k=1}^M \frac{M}{M-k}$$
