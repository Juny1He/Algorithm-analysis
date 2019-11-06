# Valid Triangle Number

## Question

Given an array consists of non-negative integers, your task is to count the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.

	Example 1:  
	Input: [2,2,3,4]  
	Output: 3
	Explanation:
	Valid combinations are: 
	2,3,4 (using the first 2)
	2,3,4 (using the second 2)
	2,2,3

Note:  

1. The length of the given array won't exceed 1000.  
2. The integers in the given array are in the range of [0, 1000].


## Solution :

This question is very similar to another question 3-Sum. For this question, we can use three pointers to solve. There are three pointers i, j, k (i < j < k). Firstly, we sort the array, which takes O(nlogn) time. Then, if nums[i] + nums[j] <= nums[k], i++, if nums[i] + nums[j] > nums[k], we can know there are j - i  combination can satisfy triangle number, then j--.  
Another thing we need to note that the edge case is when the length of array smaller than 3, return 0.

		public int triangleNumber(int[] nums) {
        if(nums.length < 3) return 0;
        Arrays.sort(nums);
        int ret = 0;
        
        for(int i = 2; i < nums.length; i ++)
        {
            int left = 0;
            int right = i - 1;
            while(left < right)
            {
                
                if(nums[left] + nums[right] > nums[i])
                {
                    ret+=right - left;
                    right--;
                }else if(nums[left] + nums[right] <= nums[i])
                {
                    left++;
                }
            }
        }
        return ret;
    }