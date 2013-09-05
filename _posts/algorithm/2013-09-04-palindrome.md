---
layout: post
title: "回文串 Palindrome"
categories: [algorithm, java]
---

这两天把Leetcode上回文串相关的三道题目(由简到难)做了，记录一下收获吧~

### Leetcode#125 Valid Palindrome

>Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

这个题目就是判断字符串是不是合法的回文序列，其中有个条件是只考虑`alphanumeric characters`且忽略大小写，注意这里是`alphanumeric`而不是`alphabetic`，开始就看错了==！

搞清楚条件，后面代码就简单了，不断找字符串前后对应的数字和字母，判断是否合法，代码如下：

    public boolean isPalindrome(String str) {
		
		for(int s=0,e=str.length()-1; s<e; s++, e--){
			while(s<e && !Character.isLetterOrDigit(str.charAt(s))) 
				s++;
			while(s<e && !Character.isLetterOrDigit(str.charAt(e)))
				e--;
			if(Character.toLowerCase(str.charAt(s)) != Character.toLowerCase(str.charAt(e)))
				return false;
		}
		
		return true;
	}

代码很短，这是参考的别人的代码，其实自己开始的思路也挺好的：

    public class ValidPalindrome {

        private static final int INVALID = -1;
        
        public int getValidCode(char c){
            
            if(c>='a' && c<='z')
                return c-'a';
            
            if( c>='A' && c<='Z')
                return c-'A';
            
            if(c>='0' && c<='9')
                return c+1000;
            
            return INVALID;
        }
        
        // 这个是最开始的思路，一步一步的判断流程，其实好多判断可以优化掉
       	public boolean isPalindrome(String str) {
            // Start typing your Java solution below
            // DO NOT write main() function
            int s=0, e=str.length()-1;
            
            while(s<e){
                
                // find first valid char
                int scode = getValidCode(str.charAt(s));
                while(scode==INVALID && s<e){
                    s++;
                    scode = getValidCode(str.charAt(s));
                }
                
                if(scode>=0){
                    // find the 'palindrome' one char
                    int ecode = getValidCode(str.charAt(e));
                    while(ecode==INVALID && s<e){
                        e--;
                        ecode = getValidCode(str.charAt(e));
                    }
                    if(ecode != scode)
                        return false;
                    else{
                        s++; e--;
                    }
                }
            }
            
            return true;
        }
        
        // 这里是对上面思路的代码优化结果，一开始写成这样就大牛了
        public boolean isPalindrome1(String str) {
            
            for(int s=0,e=str.length()-1; s<e; s++, e--){
                int scode=INVALID, ecode=INVALID;
                while(s<e && (scode = getValidCode(str.charAt(s))) == INVALID)
                    s++;
                while(s<e && (ecode = getValidCode(str.charAt(e))) == INVALID)
                    e--;
                if(scode != ecode)
                    return false;
            }
            return true;
            
        }        
        
        // 参考的别人代码
        public boolean isPalindrome2(String str) {
            
            for(int s=0,e=str.length()-1; s<e; s++, e--){
                while(s<e && !Character.isLetterOrDigit(str.charAt(s))) 
                    s++;
                while(s<e && !Character.isLetterOrDigit(str.charAt(e)))
                    e--;
                if(Character.toLowerCase(str.charAt(s)) != Character.toLowerCase(str.charAt(e)))
                    return false;
            }
            return true;
            
        }
        
        public static void main(String[] args) {
            ValidPalindrome sol = new ValidPalindrome();
            System.out.format("%s == false\n", sol.isPalindrome1("1a2"));
            System.out.format("%s == true\n", sol.isPalindrome1("A man, a plan, a canal: Panama"));
        }

    }

### Leetcode#131 Palindrome Partitioning

>Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

这个题目要求切割字符串成几个回文串，最简单最Naive的方法就是暴搜，恩，我就是dps暴搜过的～

对于字符串`abab`，以第一个字符`a`开头的回文串只有两种情况`a`,`aba`:  
    1. 当选择`a`时，递归搜索剩下的字符串`bab`;
    2. 当选择`aba`时，递归搜索剩下的字符串`b`

这就是基本思路，代码如下：

    public class PalindromePartition {
        
        // 判断子串str[sidx:eidx]是不是回文串
        public boolean isPalindrome(String str, int sidx, int eidx){
            
            while(sidx<eidx && str.charAt(sidx)==str.charAt(eidx)){
                sidx++;eidx--;
            }
            
            return sidx>=eidx;
        }
        
        // 找到所有从sidx开始的回文子串，保存其尾坐标eidx到列表中返回
        public ArrayList<Integer> findPalindromeEnds(String str, int sidx){
            
            ArrayList<Integer> res = new ArrayList<Integer>();
            
            for(int eidx = sidx; eidx<str.length(); eidx++){
                if(isPalindrome(str, sidx, eidx))
                    res.add(eidx);
            }
            
            return res;
        }
        
        // 分裂从sidx开始的子串
        public void partition(String str, Integer sidx, ArrayList<String> cur, ArrayList<ArrayList<String>> res){
            
            if(sidx>=str.length()){
                // 结束，保存结果
                res.add(new ArrayList<String>(cur));
            }else{
                for(Integer eidx : findPalindromeEnds(str, sidx))	{
                    cur.add(str.substring(sidx, eidx+1));
                    partition(str, eidx+1, cur, res); //递归分裂剩下的子串
                    cur.remove(cur.size()-1); //回朔
                }
            }
        }

        public ArrayList<ArrayList<String>> partition(String s) {
            // Start typing your Java solution below
            // DO NOT write main() function
            ArrayList<ArrayList<String>> res = new ArrayList<ArrayList<String>>();
            partition(s, 0, new ArrayList<String>(), res);
            return res;
        }

        public static void main(String[] args) {
            PalindromePartition sol = new PalindromePartition();
            String string = "cbbbcc";
            ArrayList<ArrayList<String>> res = sol.partition(string);
            for(ArrayList<String> part:res){
                System.out.println("");
                for(String str:part){
                    System.out.print(str+" ");
                }
            }
        }
    }

###Leetcode#132 Palindrome Partition 2

>Given a string s, partition s such that every substring of the partition is a palindrome. Return the minimum cuts needed for a palindrome partitioning of s.

假如这个题目没有时间复杂度要求，直接用上面的代码求得所有组合，求最短的分裂个数，但这显然是‘假如’

由于刚刚做了上面的分裂题目，思路还沉浸在暴搜中，看到这到题目，觉得在上面暴搜的基础上剪剪枝就OK了，代码如下：


    public class MinCut {

        public boolean isPalindrome(String str, int sidx, int eidx) {

            while (sidx < eidx && str.charAt(sidx) == str.charAt(eidx)) {
                sidx++;
                eidx--;
            }

            return sidx >= eidx;
        }

        public int findPalindromeEnds(String str, int sidx, int eidx) {

            while(eidx>=sidx && !isPalindrome(str, sidx, eidx)){
                eidx --;
            }

            return eidx;
        }

        public void partition(String str, Integer sidx, Counter cur, Counter minCut) {
            // 剪枝
            if (cur.val >= minCut.val)
                return;

            if (sidx >= str.length()) {
                minCut.val = cur.val;
            } else {
                for(int eidx = str.length()-1; eidx>=sidx; eidx--){
                    eidx = findPalindromeEnds(str, sidx, eidx);
                    cur.val++;
                    partition(str, eidx + 1, cur, minCut);
                    cur.val--;
                }
            }
        }

        // 暴搜+剪枝  TLE
        public int minCut(String s) {
            // Start typing your Java solution below
            // DO NOT write main() function
            Counter minCut = new Counter(Integer.MAX_VALUE);

            partition(s, 0, new Counter(0), minCut);

            return minCut.val - 1;
        }

        public static void main(String[] args) {
            Main sol = new Main();
            System.out
                    .println("cbbbcc Min Cut "
                            + sol.minCut("fifgbeajcacehiicccfecbfhhgfiiecdcjjffbghdidbhbdbfbfjccgbbdcjheccfbhafehieabbdfeigbiaggchaeghaijfbjhi"));
            System.out.println("ac Min Cut " + sol.minCut("ac"));
            System.out.println("cbcc Min Cut " + sol.minCut("cbcc"));
            System.out.println("cccc Min Cut " + sol.minCut("cccc"));
        }

        public static class Counter {
            int val = 0;

            public Counter(int val) {
                this.val = val;
            }
        }

    }

如代码中的注释，代码还是TLE了，看来必须dp啊

动态规划的思想还是很简单的，就是不断利用以前计算的结果，从而避免重复计算

额，本来想写一下算法的思想，但不知道怎么说...

`Talk is hard, see my code`

    public class MinCutDP {
        
        public boolean isPalindrome(String str, int sidx, int eidx){
            
            while(sidx<eidx && str.charAt(sidx)==str.charAt(eidx)){
                sidx++;eidx--;
            }
            
            return sidx>=eidx;
        }
        
        // 这样相当于暴搜，TLE
        public int minCut1(String s) {
            // Start typing your Java solution below
            // DO NOT write main() function
            int[] dp = new int[s.length()+1];
            dp[0] = 0;
            for(int i=1; i<dp.length; i++){
                int min = Integer.MAX_VALUE;
                for(int j=0; j<i; j++){
                    if(isPalindrome(s, j, i-1) && dp[j]<min){
                        min = dp[j];
                    }
                }
                dp[i] = min+1;
            }
            
            return dp[s.length()]-1;
        }
        
        // dp
        public int minCut(String s) {
            // Start typing your Java solution below
            // DO NOT write main() function
            // dp数组含义
            // dp[i][0]: 以s[i]结尾的最长回文子串的开始坐标
            // dp[i][1]: 子串s[0:i]的最小切割次数
            int[][] dp = new int[s.length()][2];
            dp[0][0]=0; dp[0][1]=0;
            for(int i=1; i<s.length(); i++){
                int min=Integer.MAX_VALUE, minIdx=i;
                for(int j = dp[i-1][0]-1; j<=i; j++){
                    if(j>=0 && isPalindrome(s, j, i)){
                        int cut = j>0?dp[j-1][1]+1:0;
                        if(cut<min)
                            min = cut;
                        if(j < minIdx)
                            minIdx = j;
                    }
                }
                dp[i][0] = minIdx;
                dp[i][1] = min;
            }
            
            return dp[s.length()-1][1];
        }
    }
