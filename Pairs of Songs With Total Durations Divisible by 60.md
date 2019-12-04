# Pairs of Songs With Total Durations Divisible by 60

## Question :

n a list of songs, the i-th song has a duration of time[i] seconds. 

Return the number of pairs of songs for which their total duration in seconds is divisible by 60.  Formally, we want the number of indices i < j with (time[i] + time[j]) % 60 == 0.

 

Example 1:

	Input: [30,20,150,100,40]
	Output: 3
	Explanation: Three pairs have a total duration divisible by 60:
	(time[0] = 30, time[2] = 150): total duration 180
	(time[1] = 20, time[3] = 100): total duration 120
	(time[1] = 20, time[4] = 40): total duration 60
Example 2:

	Input: [60,60,60]
	Output: 3
	Explanation: All three pairs have a total duration of 120, which is divisible by 60.
	 

Note:

1. 1 <= time.length <= 60000
2. 1 <= time[i] <= 500


## Solution : 

The Brute Force solution of this problem is traverse all the pairs with two songs, find pairs that total duration that can be divided by 60. Time complexity is O(n^2), space complexity is O(1);

However, since this problem ask us only out put number of pairs, not specific pair. Another thing we need to know that is two time duration can be a pair if and only if when they are divided by 60, the sum of their remainder can be 60. By this observation, we just need use a hashmap/ or array, to store the frequency of each remainder. There can be 60 remainder (0-59). Then remainder 1 can be paired to remainder 59 and so on. One thing need to note that remainder 0 and remainder 30 shoulde be considered independently. Time complexity is O(n), space complexity is O(60) = O(1).

	public int numPairsDivisibleBy60(int[] time) {
	        for(int i = 0; i < time.length; i ++)
	        {
	            time[i] = time[i]%60;
	        }
	        int[] a = new int[60];
	        
	        for(int i = 0; i < time.length; i++)
	        {
	            a[time[i]]++;
	        }
	        
	        int res = 0;
	        
	        for(int i = 0 ; i <= 30; i ++)
	        {
	            if(i==0 || i == 30) res+= (a[i]*(a[i] - 1))/2;
	            else 
	                res+=a[i]*a[60-i];
	        }
	        return res;
	        
	    }