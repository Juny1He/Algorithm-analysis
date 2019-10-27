# Graph Valid Tree

## Question :  

Given n nodes labeled from 0 to n-1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.  

Example 1:  

Input: n = 5, and edges = [[0,1], [0,2], [0,3], [1,4]]  
Output: true  
Example 2:  

Input: n = 5, and edges = [[0,1], [1,2], [2,3], [1,3], [1,4]]  
Output: false  
Note: you can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0,1] is the same as [1,0] and thus will not appear together in edges.  

## Solution : 

For this question, to check whether a graph is a tree, we have two things to check : 1. Whether all the vertices are connected. 2. Whether there is a cycle. 

1. To check whether all the vertices are connected, we just need to check whether the number of edges equals number of vertices minus one.

2. There are three solutions to check whether there is a cycle : 1. BFS 2. DFS 3. Union Find

1) BFS

For BFS, just start from any nodes in the graph, and record each node has been visited, if we can meet a node has been visited, there is a cycle. 

		public boolean validTree(int n, int[][] edges) {
        HashMap<Integer,HashSet<Integer>> neighbor = new HashMap<>();
        HashSet set = new HashSet();
        if(n== 1 && edges.length == 0) return true;
        if(edges.length < n-1) return false;
        for(int i = 0; i < edges.length; i ++)
        {
            int par = edges[i][0];
            int child = edges[i][1];
            
            
            if(!neighbor.containsKey(par))
            {
                neighbor.put(par,new HashSet());
            }
            neighbor.get(par).add(child);
            
            if(!neighbor.containsKey(child))
            {
                neighbor.put(child,new HashSet());
            }
            neighbor.get(child).add(par);
            
            
        }
        
        Queue<Integer> q = new LinkedList<>();
        
        q.add(edges[0][0]);
        while(!q.isEmpty())
        {
            int size = q.size();
            for(int i = 0; i < size; i ++)
            {
                int cur = q.remove();
                if(set.contains(cur)) return false;
                set.add(cur);
                for(int ch : neighbor.get(cur))
                {
                    if(set.contains(ch) && neighbor.get(ch).contains(cur)) continue;
                    q.add(ch);
                }
            }
        }
        return true;
    }


2) Union Find

For Union Find, we use a array to record every nodes' parent. Initializing this array by themselves. And traverse each edges one by one. if the edge's two nodes do not have same parent, set one node's parent as the another node. Otherwise, return false, because there are no duplicate edges, thus if they have same parent before, they can form a cycly by themselves plus their parent node.

		public boolean validTree(int n, int[][] edges) {
        int[] nums = new int[n];
        
        Arrays.fill(nums,-1);
        
        for(int i = 0; i < edges.length; i ++)
        {
            int x = find(nums,edges[i][0]);
            int y = find(nums,edges[i][1]);
            
            if(x == y) return false;
            
            nums[x] = y;
        }
        return edges.length == n - 1;
        
    	}
    	int find(int nums[], int i)
	    {
	        if(nums[i] == -1) return i;
	        return find(nums,nums[i]);
	    }
