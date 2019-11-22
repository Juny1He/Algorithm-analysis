# Diameter of Binary Tree

## Question : 

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

Example:

    Given a binary tree
              1
             / \
            2   3
           / \     
          4   5    
Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].

Note: The length of path between two nodes is represented by the number of edges between them.



## Solution :

For this question, we can found that it seems like find the sum of the left subtree's largest depth and the right subtree's largest depth. However, if the input tree is as follow, the diameter would not be that sum, but it is its left subtree's that sum.

             1
            / \
           2   3
          / \
         4   5
        /     \
       6       7
      / \       \
     8   9      10

Thus, we should discuss this problem case by case, the diameter of tree is the maximum sum of subtree's left subtree's largest depth and right subtree's largest depth. 
Thus, use a helper function to calculate the maximum depth.

    private int max = Integer.MIN_VALUE;
    public int diameterOfBinaryTree(TreeNode root) {
        helper(root);
        return max == Integer.MIN_VALUE ? 0 : max;
    }
    public int helper(TreeNode root)
    {
        if(root == null) return 0;
        int left = helper(root.left);
        int right = helper(root.right);
        max = Math.max(left+right,max);
        return Math.max(left,right)+1;
    }