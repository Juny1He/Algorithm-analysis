# Game of Life

## Question : 

According to the Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."  

Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):  

1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.  
2. Any live cell with two or three live neighbors lives on to the next generation.  
3. Any live cell with more than three live neighbors dies, as if by over-population..  
4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.  

Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.  

Example:

Input:  
[  
  [0,1,0],  
  [0,0,1],  
  [1,1,1],  
  [0,0,0]  
]  
Output:  
[  
  [0,0,0],  
  [1,0,1],  
  [0,1,1],  
  [0,1,0]  
]  
Follow up:  

1. Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.  

2. In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?  


## Solution : 

For this question, the brute force Algorithm is just record 8 cells around each cell, and change their state one by one, but we have to copy an original 2D arrays to change. However, this take O(n^2) space complexity. 

Since there is only 2 states to record death and alive. We can use 4 states to record from death to alive, death to death, alive to alive, alive to death. Then just change the cells with their original state. 

		private int m;
    private int n;
    private int[][] dir = new int[][] {{-1,0},{1,0},{0,-1},{0,1},{1,1},{-1,-1},{1,-1},{-1,1}};
    public void gameOfLife(int[][] board) {
        m = board.length;
        n = board[0].length;
        
        for(int i = 0; i < m; i ++)
        {
            for(int j = 0; j < n; j ++)
            {
                int ad = adj(board,i,j);
                if(board[i][j] == 0)
                {
                    if(ad == 3)
                    {
                        board[i][j] = 2;
                    }
                }else if(board[i][j] == 1)
                {
                    if(ad <2 || ad > 3)
                    {
                        board[i][j] = -1;
                    }
                }
            }
        }
        
        for(int i = 0; i < m; i ++)
        {
            for(int j = 0; j < n; j++)
            {
                if(board[i][j] == -1)
                {
                    board[i][j] = 0;
                }else if(board[i][j] == 2)
                {
                    board[i][j] = 1;
                }
            }
        }
        
    }
    public int adj(int[][] ori, int row, int col)
    {
        int ret = 0;
        for(int[] d : dir)
        {
            int nrow = row+d[0];
            int ncol = col+d[1];
            if(nrow < 0 || nrow >= m || ncol < 0 || ncol >= n) continue;
            if(ori[nrow][ncol] == 1 || ori[nrow][ncol] == -1)
                ret++;
        }
        return ret;
    }