# Maximum Subarray

## Question

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.  
  
Example:  

Input: [-2,1,-3,4,-1,2,1,-5,4],  
Output: 6  
Explanation: [4,-1,2,1] has the largest sum = 6.  


## Solution:  

The brute-force solution is calculate sum of each sub-array, then record the largest sum. This takes O(n^2) time. 

For dynamic programming, we need to take care of negative value, if sum < 0, we should get rid of it, and start record later subarray with sum of 0. 


	public int maxSubArray(int[] nums) {
        int sum = Integer.MIN_VALUE;
        int cur_sum = 0;
        for(int i = 0; i < nums.length; i ++)
        {
            if(cur_sum < 0)
            {
                cur_sum = 0;
            }
            cur_sum+= nums[i];
            sum = Math.max(cur_sum,sum);
        }
        return sum;
    }

Actually, a more intuitive way, is use a array OPT[i] to record the current maximum subarray with end of element nums[i]. So the recurance relation as follows:

    // OPT[i] = nums[i]             if OPT[i-1] < 0
    //        = nums[i] + OPT[i-1]  if OPT[i-1] > 0 


    public int maxSubArray(int[] nums) {      
        int[] dp = new int[nums.length];
        int max = Integer.MIN_VALUE;
        for(int i = 0; i < nums.length; i ++)
        {
            if(i == 0)
            {
                dp[i] = nums[i];
            }else
            {
                dp[i] = nums[i] + (dp[i-1] > 0 ? dp[i-1] : 0);
            }
            max = Math.max(max,dp[i]);
        }
        return max;
    }
