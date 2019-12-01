# Sliding Window Maximum

## Question:

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.  

Example:

    Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
    Output: [3,3,5,5,6,7] 
    Explanation: 

    Window position                Max
    ---------------               -----
    [1  3  -1] -3  5  3  6  7       3
     1 [3  -1  -3] 5  3  6  7       3
     1  3 [-1  -3  5] 3  6  7       5
     1  3  -1 [-3  5  3] 6  7       5
     1  3  -1  -3 [5  3  6] 7       6
     1  3  -1  -3  5 [3  6  7]      7
Note:  
You may assume k is always valid, 1 ≤ k ≤ input array's size for non-empty array.

Follow up:  
Could you solve it in linear time?


## Solution:

This Brute Force solution is find the maximum number in each sliding window, this takes O(n * k) time. 

    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0) return new int[]{};
        int[] ret = new int[nums.length - k + 1];
        int p = 0;
        for(int i = 0; i < nums.length-k+1; i ++)
        {
            int max = Integer.MIN_VALUE;
            for(int j = i; j < i+k ; j++)
            {
                max = Math.max(max,nums[j]);
            }
            ret[i] = max;
        }
        return ret;
    } 

An optimized solution is use heap and each time pop the maximum, and delete the number not in the sliding window. This takes O(n * logk).

    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0) return new int[]{};
        int[] ret = new int[nums.length - k + 1];
        int p = 0;
        for(int i = 0; i < nums.length-k+1; i ++)
        {
            int max = Integer.MIN_VALUE;
            for(int j = i; j < i+k ; j++)
            {
                max = Math.max(max,nums[j]);
            }
            ret[i] = max;
        }
        return ret;
    }

The most clever solution is use deque, at front we record the largest element's index, and each time we compare the current nums[i] to the end of the deque, if the nums[i] < the end element, we push it into the end, o.w. we pop the end element, until the nums[i] < the new end element. So that we maintain a monotonic decreasing queue. And we need to check wether the largest element is in the current sliding window by checking its index, if it is not in the current sliding window, we pop it, thus the second largest element becomes the largest. 

    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0) return new int[]{};
        int[] ret = new int[nums.length - k + 1];
        Deque<Integer> dq = new ArrayDeque<>();
        for(int i = 0; i < k; i ++)
        {
            while(!dq.isEmpty() && nums[i] > nums[dq.getLast()])
            {
                dq.pollLast();
            }
            dq.offerLast(i);
        }
        ret[0] = nums[dq.getFirst()];
        for(int i = k; i < nums.length; i ++)
        {
            if(!dq.isEmpty() && dq.getFirst() <= i - k)
            {
                dq.pollFirst();
            }
            while(!dq.isEmpty() && nums[i] > nums[dq.getLast()])
            {
                dq.pollLast();
            }
            dq.offerLast(i);
            ret[i-k+1] = nums[dq.getFirst()];
        }
        return ret;
    }
