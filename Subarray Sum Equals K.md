# Subarray Sum Equals K

## Question:

Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.

Example 1:

	Input:nums = [1,1,1], k = 2
	Output: 2

Note:
1. The length of the array is in range [1, 20,000].
2. The range of numbers in the array is [-1000, 1000] and the range of the integer k is [-1e7, 1e7].


## Solution:

The Brute Force solution is check all subarrays sum, find the number of subarrays with sum equals k. Finding all subarrays takes O(n^2), add the sum up takes O(n), thus the total time complexity is O(n^3), space complexity is O(1).

An optimized version of Brute Force is use an array to store prefix sum, so we don't need to add the sum(i,j) every time. Time complexity is O(n^2), space complexity is O(n).

An much more optimized version of Brute Force is use an variable to store the sum of current subarray(i,j). This takes O(n^2) time and O(1) space. 

The most clever solution is use hashmap to store the prefix sum and the frequency it appeard. if we find the map contains a key equals (sum - k), we can know that the subarray from that key to current sum has sum equals k. Then update the number, and update the frequency of prefix sum in map.

	public int subarraySum(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();
        int ret = 0;
        int sum = 0;
        map.put(0,1);
        for(int i = 0; i < nums.length; i ++)
        {
            sum+= nums[i];
            if(map.containsKey(sum-k))
                ret+=map.get(sum-k);
            map.put(sum,map.getOrDefault(sum,0)+1);
        }
        return ret;
    }
