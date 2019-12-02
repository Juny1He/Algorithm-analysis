# Path Sum II

## Question : 

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

Note: A leaf is a node with no children.

Example:

    Given the below binary tree and sum = 22,

          5
         / \
        4   8
       /   / \
      11  13  4
     /  \    / \
    7    2  5   1
    Return:

    [
       [5,4,11,2],
       [5,8,4,5]
    ]


## Solution :

For this question, we just need to traverse all the path of the tree, find the path that sum is equal to the target. 

One thing we need to note that we after we traverse a node, we need to remove its value. Another thing need to note when we add a list into another list, we should make a copy of it, otherwise when we make a change of this list, it would also change in the result list. 

        public List<List<Integer>> pathSum(TreeNode root, int sum) {
            List<List<Integer>> res = new ArrayList<>();
            helper(root,sum,res,new ArrayList<>());
            return res;
        }
        
        public void helper(TreeNode root, int sum, List<List<Integer>> res, List<Integer>  cur)
        {
            if(root == null) return;
            if(root.left == null && root.right == null && root.val == sum)
            {
                cur.add(root.val);
                res.add(new ArrayList<>(cur));
                cur.remove(cur.size() - 1);
                return;
            }
            
            cur.add(root.val);
            helper(root.left,sum-root.val,res,cur);
            helper(root.right,sum-root.val,res,cur);
            cur.remove(cur.size()-1);
        }