# Knight Probability in Chessboard

## Question

On an NxN chessboard, a knight starts at the r-th row and c-th column and attempts to make exactly K moves. The rows and columns are 0 indexed, so the top-left square is (0, 0), and the bottom-right square is (N-1, N-1).  

A chess knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.  

Each time the knight is to move, it chooses one of eight possible moves uniformly at random (even if the piece would go off the chessboard) and moves there.  

The knight continues moving until it has made exactly K moves or has moved off the chessboard. Return the probability that the knight remains on the board after it has stopped moving.  

Example:  

	Input: 3, 2, 0, 0  
	Output: 0.0625  
	Explanation: There are two moves (to (1,2), (2,1)) that will keep the knight on the board.  
	From each of those positions, there are also two moves that will keep the knight on the board.  
	The total probability the knight stays on the board is 0.0625.  


## Solution

The brute force solution of this problem is just record the knight position in each recurrsion, and calculate the probability. 

And a much more effcient algorithm is dynamic programming. And the state equation is as follow :  
			dp[new_row][new_col][step] = sum (dp[row_i][col_i][step-1])

	private static int[][] dir = new int[][]{{-1,-2},{1,2},{-2,-1},{2,1},{-1,2},{-2,1},{1,-2},{2,-1}};
	public static double knightProbability(int N, int K, int r, int c) {
        double[][] board = new double[N][N];
        board[r][c] = 1;
        for(int k = 0; k < K; k ++)
        {
            double[][] temp = new double[N][N];
            for(int i = 0; i < N; i ++)
            {
                for(int j = 0;j < N; j ++)
                {
                    if(board[i][j] > 0)
                    {
                        for(int[] d : dir)
                        {
                            int nrow = i+d[0];
                            int ncol = j+d[1];
                            if(nrow < 0 || nrow >= N || ncol < 0 || ncol >= N) continue;
                            temp[nrow][ncol]+=board[i][j];
                        }
                    }
                }
            }
            board = temp;
        }
        double ret = 0;
        for(int i = 0; i < N; i ++)
        {
            for(int j = 0; j < N; j ++)
            {
                ret+=board[i][j];
            }
        }
        double div = Math.pow(8,K);
        return (double) (ret) /div;
    }
