# Coin Change 2

## Question:

You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.

 

Example 1:

    Input: amount = 5, coins = [1, 2, 5]
    Output: 4
    Explanation: there are four ways to make up the amount:
    5=5
    5=2+2+1
    5=2+1+1+1
    5=1+1+1+1+1

Example 2:

    Input: amount = 3, coins = [2]
    Output: 0
    Explanation: the amount of 3 cannot be made up just with coins of 2.

Example 3:

    Input: amount = 10, coins = [10] 
    Output: 1
 

Note:

You can assume that

0 <= amount <= 5000
1 <= coin <= 5000
the number of coins is less than 500
the answer is guaranteed to fit into signed 32-bit integer

## Solution : 

For this question we can also use dynamic programming, suppose OPT(i,j) as the number of ways by using type of 1 to i coins with amount j. The recurrance relation as follows :

OPT(i,j) = OPT(i-1,j) + OPT(i,j-d). 

    public int change(int amount, int[] coins) {
    //  let OPT(i,j) as the number of ways using type of coins from 1 to i with 
    //  amount of j.

        int[][] dp = new int[coins.length+1][amount+1];
        for(int i = 0; i <= coins.length; i ++)
        {
            dp[i][0] = 1;
        }
        
        
        for(int i = 1; i <= coins.length; i ++)
        {
            for(int j = 1; j <= amount; j ++)
            {
                dp[i][j] = dp[i-1][j] + (j-coins[i-1] >= 0 ? dp[i][j-coins[i-1]] : 0);
            }
        }
        return dp[coins.length][amount];
    }


An space optimized version is use OPT(i) as the number of ways of coins combination with amount i.

OPT(i) = sum of OPT(i-d).

    public int change(int amount, int[] coins) {
        int[] dp = new int[amount+1];
        dp[0] = 1;
        for(int i = 0; i < coins.length; i ++)
        {
            for(int j = coins[i]; j <= amount; j ++)
            {
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount];
    }

One thing we can note that this code can be derived from former code. The dp[j] is the former d[i-1][j] and the dp[j] is the former d[i][j-coins[i]]. 

Another thing we need to note that the outer for loops should be coins but not amount, if we set inner loop as the coin, it will set the specific order of coin, we would have duplicate combination (e.g. with amount 3, 1+2 and 2+1 are actually the same combination with different order), so we should not set the order of coins.
