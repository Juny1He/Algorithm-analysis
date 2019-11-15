# Kth Largest Element in an Array

## Question

Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Example 1:

	Input: [3,2,1,5,6,4] and k = 2
	Output: 5
Example 2:

	Input: [3,2,3,1,2,4,5,5,6] and k = 4
	Output: 4

Note:
You may assume k is always valid, 1 ≤ k ≤ array's length.

##Solution

1. An trivial solution is just sort the array then find the Kth elements.

2. Another optimized solution is use priority queue (both min_heap and max_heap are OK), min_heap's time complexity O(nlogk) and max_heap's time complexity is O(n+klogn).

max_heap version :

	public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>(new Comparator<Integer>()
                                                        {
                                                            @Override
                                                            public int compare(Integer o1, Integer o2)
                                                            {
                                                                return o2 - o1;
                                                            }
                                                        });
        for(int i = 0; i < nums.length; i ++)
        {
            pq.offer(nums[i]);
        }
        for(int i = 0; i < k-1 ; i ++)
        {
            pq.poll();
        }
        return pq.peek();
    }

min_heap version :

	public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> q = new PriorityQueue<>();
        for(int i = 0; i < nums.length; i ++)
        {
            q.offer(nums[i]);
            if(q.size() > k)
            {
                q.poll();
            }
        }
        return q.peek();
        
    }


3. Quick Select : 

This solution is mainly based on the quick sort, each iteration find a random pivot, swap those elements which are smaller than the pivot before the pivot, and swap those larger one after the pivot, then swap the pivot into its location in sorted array. And if this location is the target, return this element, otherwise we search one of the two part splited by this location. Note: the partition part is a little bit tricky.

	public int findKthLargest(int[] nums, int k) {
        int len = nums.length; 
        int left = 0;
        int right = len - 1;
        int target = len - k;
        while(true)
        {
            int index = partition(nums,left,right);
            if(index == target)
            {
                return nums[index];
            }else if(index < target)
            {
                left = index+1;
            }else
            {
                right = index-1;
            }
        }
    }
    public int partition(int[] nums, int left, int right)
    {
        Random random = new Random();
        int piv_ind = left+ random.nextInt(right - left + 1);
        int pivot = nums[piv_ind];
        swap(nums,piv_ind,right);
        int j = left;
        for(int i = left; i <= right; i ++)
        {
            if(nums[i] < pivot)
            {
                swap(nums,i,j);
                j++;
            }
        }
        swap(nums,j,right);
        return j;
    } 
    public void swap(int[] nums,int i, int j)
    {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
