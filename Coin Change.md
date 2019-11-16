# Coin Change

## Question :

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

Example 1:

    Input: coins = [1, 2, 5], amount = 11
    Output: 3 
    Explanation: 11 = 5 + 5 + 1
Example 2:

    Input: coins = [2], amount = 3
    Output: -1
Note:
You may assume that you have an infinite number of each kind of coin.

## Solution :

Actually, this is a unbounded knapsack problem. The Brute Force solution is backtracking all the possible combination of coin denominations, and find the minimum number of coins combination. The total time complexity is O(S^n), S is the amount of the coins. 

However, we can use dynamic programming to solve this problem. The recurrance relation as follows, we assume that OPT(i) is the minimum number of coins needed according to the amount i: 

        OPT(i) = min{OPT(i - d)} + 1 
        d is the denomination

The time complexity is O(nS), space complexity is O(S).

    public int coinChange(int[] coins, int amount) {
    //  OPT(i) = min{OPT(i-di) + 1}; 
        int[] dp = new int[amount+1];
        Arrays.fill(dp,Integer.MAX_VALUE);
        dp[0] = 0;
        for(int i = 1; i <= amount; i ++)
        {
            for(int j = 0; j < coins.length; j ++)
            {
                if(i - coins[j] >= 0 && dp[i-coins[j]] != Integer.MAX_VALUE)
                {            
                    dp[i] = Math.min(dp[i],dp[i-coins[j]]+1);
                }
            }
        }
        return dp[amount] == Integer.MAX_VALUE ? -1 : dp[amount];
    }


## Follow-up :

Output the coin needed in a list

## Solution :

Since we got the minimum number of coins needed and the dynamic programming array, we can find the path we selected coins recursively. Using the recurrance relation as follows :

   while i >= 0
        if OPT(i) = OPT(i-d) + 1 :
                we add d to path
                set i = i-d
        end if
    end while
    
    public static List<Integer> path(int[] coins, int amount)
    {
        //        OPT(i) = min{OPT(i-di) + 1};
        int[] dp = new int[amount+1];
        Arrays.fill(dp,Integer.MAX_VALUE);
        dp[0] = 0;
        for(int i = 1; i <= amount; i ++)
        {
            for(int j = 0; j < coins.length; j ++)
            {
                if(i - coins[j] >= 0 && dp[i-coins[j]] != Integer.MAX_VALUE)
                {
                    dp[i] = Math.min(dp[i],dp[i-coins[j]]+1);
                }
            }
        }
        List<Integer> res = new ArrayList<>();
        int num = dp[amount];
        if(num == Integer.MAX_VALUE) return res;
        while(amount > 0)
        {
            for(int i = 0; i < coins.length; i ++)
            {
                if(amount - coins[i] >= 0 && dp[amount - coins[i]] +1 == dp[amount])
                {
                    res.add(coins[i]);
                    amount = amount - coins[i];
                    break;
                }
            }
        }
        return res;
    }

