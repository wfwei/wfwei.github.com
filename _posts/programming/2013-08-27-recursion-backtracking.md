---
layout: post
title: "递归和回朔"
category: programming
---

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


