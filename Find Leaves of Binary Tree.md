# Find Leaves of Binary Tree

## Question:

Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

 

Example:  

Input: [1,2,3,4,5]  
  
          1  
         / \
        2   3  
       / \  
      4   5      

Output: [[4,5,3],[2],[1]]  
 

Explanation:  

1. Removing the leaves [4,5,3] would result in this tree:  

          1  
         /  
        2          
 

2. Now removing the leaf [2] would result in this tree:  
 
          1           
 

3. Now removing the leaf [1] would result in the empty tree:  

          []           

## Solution:

This question is mainly about Tree Operation. There are two ways to solve this problem. 

1. The first intuitive way is remove every leaf in each iteration, until the root of this tree become null. 

		public List<List<Integer>> findLeaves(TreeNode root) {
			List<List<Integer>> res = new ArrayList<>();
			while(root != null)
			{
			    List<Integer> temp = new ArrayList<>();
			    res.add(temp);
			    root = helper(root,res);
			}
			return res;
		}

		public TreeNode helper(TreeNode root, List<List<Integer>> res)
		{
			if(root == null) return null;
			if(root.left == null && root.right == null) 
			{
			    res.get(res.size() - 1).add(root.val);
			    return null;
			}
			root.left = helper(root.left,res);
			root.right = helper(root.right,res);
			return root;
		}
In worst case, this takes O(n^2) time. 


2. The second solution is calculate layer from the bottom node each TreeNode at and add them to the according list recursively. 

		private List<List<Integer>> res = new ArrayList<>();
		public List<List<Integer>> findLeaves(TreeNode root) {
			helper(root);
			return res;
		}

		public int helper(TreeNode root)
		{
			if(root == null) return 0;
			// if(root.left == null && root.right == null) return 1;
			int level = 1 + Math.max(helper(root.left), helper(root.right));
			if(res.size() < level) res.add(new ArrayList<>());
			res.get(level-1).add(root.val);
			return level;
		}
This takes O(n) time, since we just traverse each TreeNode once. 





