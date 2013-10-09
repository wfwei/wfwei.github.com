---
layout: post
title: "递归和回朔"
categories: [algorithm, recursive]
---

### 递归

使用递归的最常见例子就是求解斐波那契数列，因为斐波那契数列本身就是以递归的方法定义的：

> F(0)=0，F(1)=1，F(n)=F(n-1)+F(n-2)（n>=2，n∈ N*）

使用递归的简单实现如下：

    int fib(int n){
        if(n<2)
            return n;
        return fib(n-1) + fib(n-2);
    }

代码很直观，但是存在大量重复计算，主要是因为在计算`f(n)`的时候要计算`f(n-2)`，而在计算`f(n-1)`的时候还要重复再计算一次`f(n-2)`

优化的方法，或者说去除重复计算的方法，最直接的就是保留中间结果，即第一次计算`f(n-2)`后保存结果，方便下次使用

优化后代码：

    int fib_seq[MAX]; // global variable, store fibonacci sequence

    int fib(int n){
        if(n<2) return n;
        if(fib_seq[n] != 0) return fib_seq[n]; // reuse last result
        return fib_seq[n]=fib(n-1)+fib(n-2);
    }



下面的递归程序是巧妙运用栈空间的例子：

>将1->2->3->4->5->6->7 变成 1->7->2->6->3->5->4，recursive解法

    Node start;
    private void rev(Node end) {
		if (end.next != null)
			rev(end.next); // find the last node
		
		// visit all 'last nodes'
		if(start!=null){ //not over
			if (start == end || start.next == end) { // first over
				end.next = null; // end the chain
				start=null; // mark as over
			} else {
				// insert node
				end.next = start.next;
				start.next = end;
				start = end.next;
			}	
		}
	}

完整代码见[这里](https://gist.github.com/wfwei/6839936)

该题是微软2014年校招的笔试题目，当时我没有使用递归，大致思路是：

1. 使用快慢指针，找到链表中间节点，将链表一份为二
2. 对后半部分反转
3. 交叉‘归并’前后两部分


### 回朔

下面程序是数读求解的核心代码，里面用到了递归和回朔以及终止回朔，详见注释～

全部代码参考[这里](https://github.com/wfwei/coding/blob/master/leetcode/35-sudoku.java)，题目来自[Leetcode](http://leetcode.com/onlinejudge#question_37)

需要格外注意的是： 带返回值的递归函数尤其要注意每个结束分支的返回值状态

    private boolean solve(char[][] board, int row, int col) {

		if (col >= board.length) {
			col = 0;
			row++;
		}
		if (row >= board.length){
			return true;
		}

		if (board[row][col] == '.') {
			int[] nums = collectNums(board, row, col);
			for (int i = 1; i < 10; i++) {
				if (nums[i] > 0)
					continue;
				board[row][col] = (char) (i + '0');
                // 通过递归函数的返回状态确定是否回朔
				if(solve(board, row, col + 1))
					return true;
				board[row][col] = '.';
			}
			return false;
		} else {
			return solve(board, row, col + 1);
		}
	}


