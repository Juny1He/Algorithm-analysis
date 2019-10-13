# 312. Burst Balloons

## Question  
Hard

Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.  

Note:

You may imagine nums[-1] = nums[n] = 1. They are not real therefore you can not burst them.  
0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100  

Example:

Input: [3,1,5,8]  
Output: 167  
Explanation: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []  
             coins =  3 * 1 * 5      +  3 * 5 * 8    +  1 * 3 * 8      + 1 * 8 * 1   = 167  

## Solution:

For this question the subproblems are overlapped. So we can use divide and conquer + dynamic programming.

If we burst the i-th balloon at the start, we will get coins nums[i-1]*nums[i]*nums[i+1]. If we burst the i-th balloon at the end, we will get coins nums[-1]*nums[i]*nums[n].  
If we consider i is the last one to burst, then we can see that balloons are separated into 2 sections, the left and right section do not affect each other. We can get the recurrance relation as follow :  
OPT[start, end] = Max(OPT[start,i-1] + coins[i] + OPT[i+1,end]). (start < i < end).

	public int maxCoins(int[] nums) {
        int[][] dp = new int[nums.length][nums.length];
        return maxCoins(nums,0,nums.length - 1, dp);
    }
    
    public int maxCoins(int[] nums, int start, int end, int[][] dp)
    {
        if(start > end) return 0;
        
        if(dp[start][end] != 0)
            return dp[start][end];
        
        int max = 0;
        for(int i = start; i <= end; i ++)
        {
            int val = maxCoins(nums,start, i-1,dp)+get(nums,i)*get(nums,start-1)*get(nums,end+1) + maxCoins(nums,i+1,end,dp);
            
            max = Math.max(max,val);
        }
        
        dp[start][end] = max;
        return max;
    }
    
    public int get(int[] nums,int i)
    {
        if(i == -1 || i == nums.length)
            return 1;
        return nums[i];
    }
