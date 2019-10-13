# 1222. Queens That Can Attack the King

## Question
Medium

On an 8x8 chessboard, there can be multiple Black Queens and one White King. Given an array of integer coordinates queens that represents the positions of the Black Queens, and a pair of coordinates king that represent the position of the White King, return the coordinates of all the queens (in any order) that can attack the King.
 
Example 1:

Input: queens = [[0,1],[1,0],[4,0],[0,4],[3,3],[2,4]], king = [0,0]
Output: [[0,1],[1,0],[3,3]]

Explanation: 

The queen at [0,1] can attack the king cause they're in the same row.
The queen at [1,0] can attack the king cause they're in the same column. 
The queen at [3,3] can attack the king cause they're in the same diagnal. 
The queen at [0,4] can't attack the king cause it's blocked by the queen at [0,1]. 
The queen at [4,0] can't attack the king cause it's blocked by the queen at [1,0]. 
The queen at [2,4] can't attack the king cause it's not in the same row/column/diagnal as the king.

Example 2:

Input: queens = [[0,0],[1,1],[2,2],[3,4],[3,5],[4,4],[4,5]], king = [3,3]
Output: [[2,2],[3,4],[4,4]]
Example 3:

Input: queens = [[5,6],[7,7],[2,1],[0,7],[1,6],[5,1],[3,7],[0,3],[4,0],[1,2],[6,3],[5,0],[0,4],[2,2],[1,1],[6,4],[5,4],[0,0],[2,6],[4,5],[5,2],[1,4],[7,5],[2,3],[0,5],[4,2],[1,0],[2,7],[0,1],[4,6],[6,1],[0,6],[4,3],[1,7]], king = [3,4]
Output: [[2,3],[1,4],[1,6],[3,7],[4,3],[5,4],[4,5]]
 
Constraints:
•1 <= queens.length <= 63  
•queens[0].length == 2  
•0 <= queens[i][j] < 8  
•king.length == 2  
•0 <= king[0], king[1] < 8  
•At most one piece is allowed in a cell.  


## Solution:

We can check whether each queen is on the same row or the same column or the same diagonal with the king. Thus, there are 8 directions we can check. Since a queen can be blocked by other queen, we can run BFS from king, until all the positions are checked. We can use 8 boolean variables to record on each direction whether there is a queen can kill the king, if there is a such queen, the boolean variable becomes true and we don’t check that direction any more, since other queen on this direction would be blocked by such a queen.

JAVA:
	
	public List<List<Integer>> queensAttacktheKing(int[][] queens, int[] king) {
	
        HashSet<Integer> set = new HashSet();
        List<List<Integer>> result = new ArrayList<>();
		//8 directions
        int[][] dir = new int[][]{{-1, 0}, {-1, 1}, {0, 1}, {1, 1}, {1, 0}, {1, -1}, {0, -1}, {-1, -1}};
        boolean[] rec = new boolean[8];
        for (int[] cur : queens) {
            set.add(cur[0] * 8 + cur[1]);
        }
	//      If the number of queen who can kill the king is 8, we stop the algorithm to save running time.
        int total = 0;
        int row = king[0];
        int col = king[1];
        for (int i = 1; i <= 7; i++) {
            for (int j = 0; j < 8; j++) {
                int x = row + dir[j][0]*i;
                int y = col + dir[j][1]*i;
                if((0<= x && x <= 7) && (0<= y && y <= 7))
                if (!rec[j] && total <= 8)
                {
                    int cur = x * 8 + y;
                    if (set.contains(cur)) {
                        List<Integer> temp = new ArrayList<>();
                        temp.add(x);
                        temp.add(y);
                        result.add(temp);
                        rec[j] = true;
                        total++;
                    }
                }
            }
        }
        return result;
    }

