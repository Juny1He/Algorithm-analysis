# Longest Palindromic Subsequence

## Question:

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.

Example 1:  
Input:  

"bbbab"  
Output:  
4  
One possible longest palindromic subsequence is "bbbb".  
Example 2:  
Input:  

"cbbd"  
Output:  
2  
One possible longest palindromic subsequence is "bb".  

## Solution:

This question is a classic dynamic programming problem. We can divide the whole problem into subproblems. Let OPT(i,j) be the maximum numbers of character of palindrome of the s.substring(i,j+1). Then, if character i is same as character j. The OPT(i,j) is equal to OPT(i+1,j-1)+2 (two character of i and j). Otherwise, the OPT(i,j) is equal to max(OPT(i+1,j),OPT(i,j-1)). The initial condition is if i is same as j, the OPT(i,j) is equal to 1 (there is only one character form a palindrome).

        public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];
        
        for(int i = s.length() - 1; i >= 0; i -- )
        {
            for(int j = i; j < s.length(); j ++)
            {
                if(i == j ) dp[i][j] = 1;
                else if(s.charAt(i) == s.charAt(j))
                {
                    dp[i][j] = dp[i+1][j-1] + 2;
                }else
                {
                    dp[i][j] = Math.max(dp[i+1][j],dp[i][j-1]);
                }
            }
        }
        return dp[0][n-1];
    }

