# Friend Circles  


## Question

There are N students in a class. Some of them are friends, while some are not. Their friendship is transitive in nature. For example, if A is a direct friend of B, and B is a direct friend of C, then A is an indirect friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.  

Given a N * N matrix M representing the friend relationship between students in the class. If M[i][j] = 1, then the ith and jth students are direct friends with each other, otherwise not. And you have to output the total number of friend circles among all the students.    

Example 1:  

	Input: 
	[[1,1,0],
	 [1,1,0],
	 [0,0,1]]
	Output: 2
	Explanation:The 0th and 1st students are direct friends, so they are in a friend circle. 
	The 2nd student himself is in a friend circle. So return 2.

Example 2:

	Input: 
	[[1,1,0],
	 [1,1,1],
	 [0,1,1]]
	Output: 1
	Explanation:The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
	so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.
Note:  
1. N is in range [1,200].  
2. M[i][i] = 1 for all students.
3. If M[i][j] = 1, then M[j][i] = 1.

## Solution

For this question, we can solve it by dfs, search ith people and its friends, and its friends' friends until all friends are searched we end dfs, and start to find other friend cycle in people unvisited. We should need a visited array to record which people are visited which are not. 

	public int findCircleNum(int[][] M) {
        boolean[] visited = new boolean[M.length];
        int count = 0;
        for(int i = 0; i < M.length; i ++)
        {
            if(!visited[i])
            {
                dfs(M,visited,i);
                count++;
            }
        }
        return count;
    }
    public void dfs(int[][] M, boolean[] visited, int person)
    {
        for(int other = 0; other < M.length; other++)
        {
            if(M[person][other] == 1 && !visited[other])
            {
                visited[other] = true;
                dfs(M,visited,other);
            }
        }
    }


Another solution is use union find, at beginning, we set a array to record every people's uppermost parent, initialize the value of their parent as themselves. Then, search every people and find their uppermost parent, if ith people and jth people are friends, we can union their uppermost parent to one. Finally, we just need to find how much people's uppermost parent are themslves which means how much friend circles exist. An optimalization of union find is path compression, set every people's parent is their uppermost parent, so we do not need to use to much loops to find their uppermost parent one by one.
	
	public int findCircleNum(int[][] M) {
        int[] a = new int[M.length];
        int ret = 0;
        for(int i = 0; i < a.length; i ++)
        {
            a[i] = i;
        }
        for(int i = 0; i < M.length; i ++)
        {
            for(int j = i+1; j < M.length; j++)
            {
                if(M[i][j] == 1)
                {
                    union(a,i,j);
                }
            }
        }
        
       
        
        for(int i = 0; i < a.length; i ++)
        {
            if(a[i] == i) ret++;
        }
        
        return ret;
    }
    public void union (int[] a, int i, int j)
    {
        while(a[i] != i) i = a[i];
        while(a[j] != j) j = a[j];
        if(a[i] != a[j]) a[i] = j;
    }