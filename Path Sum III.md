# Path Sum III

## Question : 

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

Example:

	root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

	      10
	     /  \
	    5   -3
	   / \    \
	  3   2   11
	 / \   \
	3  -2   1

	Return 3. The paths that sum to 8 are:

	1.  5 -> 3
	2.  5 -> 2 -> 1
	3. -3 -> 11


## Solution : 

For this problem, the Brute Force is set every node as root, and traverse all the path for every root, time complexity is O(n^2) (for balanced tree, the time complexity is O(nlogn)), space complexity is O(n);

One thing need to note that we should use a helper function to do this, the original function is to set every node as root, the helper function is to count the number of path. 

	 	public int pathSum(TreeNode root, int sum) {
	        if(root == null) return 0;
	        return helper(root,sum)+ pathSum(root.left,sum)+ pathSum(root.right,sum);
	    }
	    
	    public int helper(TreeNode root, int sum)
	    {
	        if(root == null) return 0;
	        return (root.val == sum ? 1 : 0) + helper(root.left,sum-root.val) + helper(root.right,sum-root.val);
	    }


A better solution is to use a hashmap to store the prefixSum and its frequency, and we just need to calculate whether there is a key in hashmap that satisfied currSum - target == key, if it satisfied we can update the number of path. One thing we need to note that after we traversing a subtree, we should remove the prefixSum in that subtree. The time complexity is O(n).

		int count = 0;
	    public int pathSum(TreeNode root, int sum) {
	        HashMap<Integer,Integer> map = new HashMap<>();
	        map.put(0,1);
	        helper(root,0,sum,map);
	        return count;
	    }
	    
	    public void helper(TreeNode root, int currSum, int target, HashMap<Integer,Integer> map)
	    {
	        if(root == null) return;
	        
	        currSum += root.val;
	        
	        if(map.containsKey(currSum - target))
	        {
	            count+=map.get(currSum-target);
	        }
	        
	        map.put(currSum,map.getOrDefault(currSum,0)+1);
	        
	        helper(root.left,currSum,target,map);
	        helper(root.right,currSum,target,map);
	        map.put(currSum,map.get(currSum)-1);
	    }






