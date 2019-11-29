# Next Greater Element I

## Question:

You are given two arrays (without duplicates) nums1 and nums2 where nums1’s elements are subset of nums2. Find all the next greater numbers for nums1's elements in the corresponding places of nums2.

The Next Greater Number of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, output -1 for this number.

Example 1:

    Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
    Output: [-1,3,-1]
    Explanation:
        For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
        For number 1 in the first array, the next greater number for it in the second array is 3.
        For number 2 in the first array, there is no next greater number for it in the second array, so output -1.

Example 2:

    Input: nums1 = [2,4], nums2 = [1,2,3,4].
    Output: [3,-1]
    Explanation:
        For number 2 in the first array, the next greater number for it in the second array is 3.
        For number 4 in the first array, there is no next greater number for it in the second array, so output -1.
Note:
All elements in nums1 and nums2 are unique.
The length of both nums1 and nums2 would not exceed 1000.


## Solution: 

1. The Brute Force solution of this problem is search every next greater element of nums1's element in nums2. An optimized way is use a hash map to store the index of nums1's element in nums2. The total time complexity is O(n^2).

    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i = 0; i < nums2.length; i ++)
        {
            map.put(nums2[i],i);
        }
        int[] ret = new int[nums1.length];
        Arrays.fill(ret,-1);
        for(int i = 0; i < nums1.length; i ++)
        {
            for(int j = map.get(nums1[i]); j < nums2.length; j ++)
            {
                if(nums2[j] > nums1[i])
                {
                    ret[i] = nums2[j];
                    break;
                }
            }
        }
        return ret;
    }


2. Another clever solution is traverse the nums2 and use a hashmap to record next greater element of nums2's element in nums2. Then, we can know the next greater element of nums1's element. For this problem, we can use a stack to traverse the nums2, the stack's element in a non-increasing order, if we find the nums2[i] > stack's top element, we know that the nums2[i] is the next greater element of top element and pop the top element, push the nums2[i] into stack, o.w. store the nums2[i] into stack.

    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        if(nums2.length == 0) return nums2;
        HashMap<Integer,Integer> map = new HashMap<>();
        Stack<Integer> st = new Stack();
        st.push(nums2[0]);
        for(int i = 1; i < nums2.length; i ++)
        {
            while(!st.isEmpty() && nums2[i] > st.peek())
            {
                map.put(st.pop(),nums2[i]);
            }
            st.push(nums2[i]);
        }
        while(!st.isEmpty())
        {
            map.put(st.pop(),-1);
        }
        int[] ret = new int[nums1.length];
        for(int i = 0; i < nums1.length; i ++)
        {
            ret[i] = map.get(nums1[i]);
        }
        return ret;
    } 