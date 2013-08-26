---
layout: post
title: "背包问题"
categories: [programming, algorithm, 背包问题]
---

背包问题，[背包九讲](http://cuitianyi.com/Pack/P01.html)的笔记，所有代码参见我的[Gist](https://gist.github.com/wfwei/6342360)

### 01背包问题

**问题:**

N个物品和一个容量为V的背包。第i件物品的费用是c[i]，价值是w[i]。求解将哪些物品装入背包可使价值总和最大，求最大价值。

每种物品仅有一件，可以选择放或不放。

**基本思路：**

背包问题是要选择N个物品的子集放到容量为V的背包，求解思路是求解其子问题：将前i(0<=i<=N)件物品放入容量为j(0<=j<=V)的背包可以获得的最大价值

* 背包容量为V，可以看作有容量分别为`{0,1,...,V}`的V+1种状态，计算每个状态下背包的最大价值
* 物品个数为N，可以先计算N-1个物品的情况，之后，使用状态转移方程计算N个物品的问题

将前i件物品放入容量为j的背包可以获得的最大价值有两种情况：

1. 不包括第i个物品，这时候最大价值为前i-1个物品放到容量为j的背包的最大价值，即`f[i-1][j]`
2. 包括第i个物品，只是最大价值为前i-1个物品放到容量为j的背包的最大价值(`f[i-1][v-c[i-1]]`)加上第i个物品的价值(`w[i]`)，即`f[i-1][v-c[i-1]]+w[i]`

使用`f[i][j]`表示前i件物品恰放入一个容量为j的背包可以获得的最大价值，则转移方程为： 

` f[i][j] = max{ f[i-1][j], f[i-1][v-c[i]]+w[i] } `

核心代码如下：

    int ZeroOnePack(int N, int V, int c[], int w[]) {
		int **f = malloc2dArray(N + 1, V + 1);
		int i, j;
		for (i = 1; i <= N; i++) {
			for (j = 1; j <= V; j++) {
				// c[i-1],w[i-1]是因为i是从1开始的
				if (j >= c[i - 1])
					f[i][j] = max(f[i - 1][j],
							f[i - 1][j - c[i - 1]] + w[i - 1]);
				else
					f[i][j] = f[i - 1][j];
			}
		}
		int res = f[N][V];
		free(f);
		return res;
	}

上面描述的算法的时间和空间复杂度都是`O(VN)`，其中时间复杂度应该已经不能再优化了，但空间复杂度却可以优化到O(N)。

从上面代码思路可以看出，计算`f[i][:]`只有到了`f[i-1][:]`，所以使用一维数组`f[V]`保存每次循环后的状态就可以了。
`f[j] (j=1,2,..,V)`表示前i个物品的最大价值，对于第i+1个物品，转移方程为：`f[j]=max{f[j], f[j-c[i]]+w[i]}`，我们看到，计算新的`f[j]`用到了`f[j-c[i]]`，我们要求`f[j-c[i]]`是上一轮的值，不能被更新过，所以我们需要逆序推`f[j]`，即：

    for i=1...N
        for j=V...0
            f[j]=max{f[j],f[j-c[i]]+w[i]};

核心代码如下：

    int ZeroOnePackPro(int N, int V, int c[], int w[]) {
		int *f = (int *) malloc(sizeof(int) * (V + 1));
		memset(f, 0, sizeof(int) * (V + 1));
		int i, j;
		for (i = 1; i <= N; i++) {
			for (j = V; j >= c[i - 1]; j--) {
				f[j] = max(f[j], f[j - c[i - 1]] + w[i - 1]);
				// c[i-1],w[i-1]是因为i是从1开始的
			}
		}
		int res = f[V];
		free(f);
		return res;
	}

扩展，如果题目要求“恰好装满背包”时的最优解，这时候需要将f初始化为负无穷；如果没要求必须装满背包，则初始化为

### 完全背包问题

**问题:**

N个物品和一个容量为V的背包，每种物品都有无限件可用。第i件物品的费用是c[i]，价值是w[i]。求解将哪些物品装入背包可使价值总和最大，求最大价值。

**基本思路：**

完全背包问题和01背包问题很相似，唯一不同就在于每个物品的状态不是要1个还是0个，而是k个，k>=0，这样套用01背包的分析得转化方程为：

` f[i][j] = max{ f[i-1][j], f[i-1][v-k*c[i]]+k*w[i] } `

>仔细分析，假如要物品`i`，说明物品i在当前背包容量`v`下是性价比`(w[i]/c[i])`最高的，所以如果要则会要`k=v/c[i]`(向下取整)个

上面的‘仔细分析’就错了，这种‘贪心’的做法有时候取不到最优值，举个例子，`N=2, V=28, c=[10, 9], w[21, 18]`，加入使用这种贪心的做法，由于物品1的‘性价比’是`21/10=2.1`，大于物品2的`18/9=2`，所以会选择`k=28/10=2`个物品1,`0`个物品2,最后的总价值是`42`，而真正的最优解是选择`1`个物品1,`2`个物品2,最优值是`57`。

这里思路虽然和01背包很相似，但是这个k值在每次迭代中都是无法确定的，选择当前最优的贪心做法往往取不到最优值，如上所述

其实，完全背包很容易转化为01背包，只要让完全背包中的每个物品组多只能要1次即可，由于完全背包中不限次数，但背包总容量一定，每个物品做多被要的次数`n[i]<=V/c[i]`，这样，将背包中的每个物品重复`n[i]`次即可转化为普通背包问题，这种思路的复杂度是`O(V*Σ(V/c[i]))`，这个是挺大的，有种思路可以降低复杂度到`O(V*logΣ(V/c[i]))`，具体思路在下面的多重背包问题中介绍吧，这里先给出基本解法：

    int CompletePack(int N, int V, int c[], int w[]) {
		int **f = malloc2dArray(N + 1, V + 1);
		int i, j, k;
		for (j = 1; j <= V; j++) {
			for (i = 1; i <= N; i++) {
				// c[i-1],w[i-1]是因为i是从1开始的
				k = j / c[i - 1];
				while (k >= 0) {
					int res =  max(f[i - 1][j],
							f[i - 1][j - k * c[i - 1]] + k * w[i - 1]);
					f[i][j] = max(res, f[i][j]);
					k--;
				}
			}
		}
		int res = f[N][V];
		free(f);
		return res;
	}

和01背包解法类似，完全背包问题也有优化方案，可以将空间复杂度降到`O(N)`

    for i=1..N
        for j=0..V
            f[j]=max{f[j],f[j-cost]+weight}

和01背包中不同的地方在于不用逆序了，这个地方挺巧妙的，不太容易理解透亮--!

核心代码

    int CompletePackPro(int N, int V, int c[], int w[]) {
		int *f = (int *) malloc(sizeof(int) * (V + 1));
		memset(f, 0, sizeof(int) * (V + 1));
		int i, j;
		for (i = 1; i <= N; i++) {
			for (j = c[i - 1]; j <= V; j++) {
				f[j] = max(f[j], f[j - c[i - 1]] + w[i - 1]);
				// c[i-1],w[i-1]是因为i是从1开始的
			}
		}
		int res = f[V];
		free(f);
		return res;
	}

###多重背包问题

**问题:**

有N种物品和一个容量为V的背包。第i种物品最多有n[i]件可用，每件费用是c[i]，价值是w[i]。求解将哪些物品装入背包可使这些物品的费用总和不超过背包容量，且价值总和最大。

**基本思路：**

多重背包又和完全背包问题很相似，唯一不同就在于每个物品可以取的次数限制成`n[i]`了，完全背包中讲到的关于如何转化为01背包的思路完全适用～～

上面说道转化为01背包的方法可以改进，基本思想是：物品i可以重复n[i]次，简单的思路是放n[i]个物品i到物品集合中，这样每个物品i最多被选择1次，满足01背包的要求，现在我们可以优化这里的n[i]，少放几个物品i降低复杂度

具体思路是将物品i扩展成若干物品，每个物品的费用和价值分别为`c[i]*2^k, w[i]*2^k`，其中`c[i]*2^k<=V`，假设`c[i]=3, w[i]=4, n[i]=13` ，我们只需将`2^k=1,(2,3)`，`2^k=2,(4,6)`，`2^k=4,(8,12)`，`13-1-2-4=6,(12,18)`四个物品添加到物品集合中，就能表示n[i]个物品i的所有组合

这样，多重背包的复杂度为`O(V*Σn[i])`

    procedure MultiplePack(cost,weight,amount)
        if cost*amount>=V
            CompletePack(cost,weight)
            return
        integer k=1
        while k<amount
            ZeroOnePack(k*cost,k*weight)
            amount=amount-k
            k=k*2
        ZeroOnePack(amount*cost,amount*weight)

核心解法是:

    // Pass through hdu2191
	int MultiplePackPro(int N, int V, int c[], int w[], int n[]) {
		int *f = (int *) malloc(sizeof(int) * (V + 1));
		memset(f, 0, sizeof(int) * (V + 1));
		int i, j;
		for (i = 1; i <= N; i++) {
			if (n[i - 1] * c[i - 1] >= V) {
				for (j = c[i - 1]; j <= V; j++) {
					f[j] = max(f[j], f[j - c[i - 1]] + w[i - 1]);
				}
			} else {
				int k = 1, amount = n[i - 1];
				while (k < amount) {
					for (j = V; j >= k * c[i - 1]; j--) {
						f[j] = max(f[j], f[j - k * c[i - 1]] + k * w[i - 1]);
					}
					amount -= k;
					k += k;
				}
				k = amount;
				for (j = V; j >= k * c[i - 1]; j--) {
					f[j] = max(f[j], f[j - k * c[i - 1]] + k * w[i - 1]);
				}
			}
		}
		int res = f[V];
		free(f);
		return res;
	}
