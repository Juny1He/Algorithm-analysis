# Find First and Last Position of Element in Sorted Array

## Question  

Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.  

Your algorithm's runtime complexity must be in the order of O(log n).  

If the target is not found in the array, return [-1, -1].  

Example 1:  

	Input: nums = [5,7,7,8,8,10], target = 8
	Output: [3,4]
	Example 2:
Example 2:  

	Input: nums = [5,7,7,8,8,10], target = 6
	Output: [-1,-1]

## Solution:

For this question, the brute force solution is just search the left boundary and right boundary linearly, run two pass for loops. This takes O(2n) = O(n);

	public int[] searchRange(int[] nums, int target) {
	        int left = -1;
	        int right = -1;
	        for(int i = 0; i < nums.length; i ++)
	        {
	            if(nums[i] == target)
	            {
	                left = i;
	                break;
	            }
	        }
	        
        for(int i = nums.length - 1; i >= 0; i --)
        {
            if(nums[i] == target)
            {
                right = i;
                break;
            }
        }
        
        return new int[]{left,right};
    }

However, this input array is in ascending order, thus we can search the left boundary and right boundary with binary search. One thing need to know is we should process the edge case carefully, when we find the middle number is target, we should check whether its neighbors are also the target and avoid the situation that the middle number is on the right or left corner so that it only has one neighbor.   

	public int[] searchRange(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        int p1 = -1;
        
        while(left <= right)
        {
            int mid = (left - right)/2 + right;
            if(nums[mid] == target)
            {
                if(mid > 0 && nums[mid] == nums[mid-1])
                {
                    right = mid - 1;
                }else 
                {
                    p1 = mid;
                    break;
                }
            }else if(nums[mid] < target)
            {
                left = mid + 1;
            }else 
            {
                right = mid - 1;
            }
        }
        
        int p2 = -1;
        left = 0;
        right = nums.length - 1;
        while(left <= right)
        {
            int mid = (left - right)/2 + right;
            if(nums[mid] == target)
            {
                if(mid < nums.length-1 && nums[mid] == nums[mid+1])
                {
                    left = mid + 1;
                }else 
                {
                    p2 = mid;
                    break;
                }
            }else if(nums[mid] < target)
            {
                left = mid + 1;
            }else 
            {
                right = mid - 1;
            }
        }
        return new int[]{p1,p2};
    }




