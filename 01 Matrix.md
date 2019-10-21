# 01 Matrix

## Question:  

Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell.  

The distance between two adjacent cells is 1.  

 

Example 1:  
  
Input:  
[[0,0,0],  
 [0,1,0],  
 [0,0,0]]  

Output:  
[[0,0,0],  
 [0,1,0],  
 [0,0,0]]  

Example 2:  

Input:  
[[0,0,0],  
 [0,1,0],  
 [1,1,1]]  

Output:  
[[0,0,0],  
 [0,1,0],  
 [1,2,1]]  
 

Note:  

1. The number of elements of the given matrix will not exceed 10,000.  
2. There are at least one 0 in the given matrix.  
3. The cells are adjacent in only four directions: up, down, left and right.  

## Solution:

For this question, the basic idea is to use BFS, and run BFS from square with 1, and find the shortest path from 1 to 0. However, this is relatively low-efficient, because if there are a lot of 1, we have to search path from these 1 to nearest 0 one by one. Therefore, we can run BFS from another way : from 0 to nearest 1. 

		public int[][] updateMatrix(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        for(int i = 0; i < m; i ++)
        {
            for(int j = 0; j < n; j++)
            {
                if(matrix[i][j] != 0)
                {
                    matrix[i][j] = Integer.MAX_VALUE;
                }
            }
        }
        
        for(int i = 0; i < m; i ++)
        {
            for(int j = 0; j < n; j++)
            {
                if(matrix[i][j] == 0) continue;
                else
                {
                    matrix[i][j] = findmin(matrix[i-1][j],matrix[i][j+1],matrix[i+1][j],matrix[i][j-1])+1;
                }
            }
        }
        return matrix;
    }