# Binary Tree Level Order Traversal


## Question  
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).  

For example:  
Given binary tree [3,9,20,null,null,15,7],  
   
   3  
  / \  
 9  20  
   /  \  
  15   7  
return its level order traversal as:  
[  
  [3],  
  [9,20],  
  [15,7]  
]  



## Solution : 

This question have two solutions : 1. Iteration 2. Recurrsion.

For Iteration, we have to use queue to record every parent node in each iteration.

	public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if(root == null) return res;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while(!q.isEmpty())
        {
            int size = q.size();
            List<Integer> temp = new ArrayList<>();
            for(int i = 0; i < size; i ++)
            {
                TreeNode cur = q.remove();
                temp.add(cur.val);
                if(cur.left != null)
                q.add(cur.left);
                if(cur.right != null)
                q.add(cur.right);
            }
            res.add(temp);
        }
        return res;
    }


For Recurrsion, we have to record which layer current node is located in, and add its value to the according list. 

	public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        helper(result,root,0);
        return result;
    	}
    
	    public void helper(List<List<Integer>> result, TreeNode root, int depth)
	    {
		if(root == null) return;
		if(result.size() == depth)
		{
		    List<Integer> temp = new ArrayList<>();
		    temp.add(root.val);
		    result.add(temp);
		}else
		{
		    result.get(depth).add(root.val);
		}
		helper(result,root.left,depth+1);
		helper(result,root.right,depth+1);

	    }
