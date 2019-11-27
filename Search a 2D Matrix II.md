# Search a 2D Matrix II


## Question

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:  

Integers in each row are sorted in ascending from left to right.  
Integers in each column are sorted in ascending from top to bottom.  

Example:  

Consider the following matrix:

	[
	  [1,   4,  7, 11, 15],
	  [2,   5,  8, 12, 19],
	  [3,   6,  9, 16, 22],
	  [10, 13, 14, 17, 24],
	  [18, 21, 23, 26, 30]
	]

Given target = 5, return true.

Given target = 20, return false.


## Solution

1 For this question, the Brute Force solution is just search every element in the matrix, this time complexity is O(mn).

2 Another straightforward solution is use divide and conquer, we divide the original matrix into four parts and if the target == middle one we return true, if the target < middle element, we discard the zone 4, otherwise discard the zone 1. So the recurance relation is T(n) = 3T(n/4) + O(1). The time complexity is O((mn)^log4(3)).

		  zone 1      zone 2
		*  *  *  * | *  *  *  *
		*  *  *  * | *  *  *  *
		*  *  *  * | *  *  *  *
		*  *  *  * | *  *  *  *
		-----------------------
		*  *  *  * | *  *  *  *
		*  *  *  * | *  *  *  *
		*  *  *  * | *  *  *  *
		*  *  *  * | *  *  *  *
		  zone 3      zone 4


	public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
        if(m == 0) return false;
        int n = matrix[0].length;
        return dandc(matrix,0,0,m-1,n-1,target);
    }
    public boolean dandc(int[][] matrix, int up_row, int up_col, int down_row, int down_col, int target)
    {
        if(up_row > down_row || up_col > down_col) return false;
        if(target < matrix[up_row][up_col] || target > matrix[down_row][down_col]) return false;
        int mid_row = (up_row+down_row) >>> 1;
        int mid_col = (up_col+down_col) >>> 1;
        if(target == matrix[mid_row][mid_col]) return true;
        else if(target > matrix[mid_row][mid_col])
        {
            return dandc(matrix,up_row,mid_col+1,mid_row,down_col,target) ||
                   dandc(matrix,mid_row+1,mid_col+1,down_row,down_col,target) ||
                   dandc(matrix,mid_row+1,up_col,down_row,mid_col,target);
        }else
        {
            return dandc(matrix,up_row,mid_col+1,mid_row,down_col,target) ||
                   dandc(matrix,up_row,up_col,mid_row,mid_col,target) ||
                   dandc(matrix,mid_row+1,up_col,down_row,mid_col,target);
        }
        
    }

3 The most clever solution is as follow : we can observe that start from the left bottom element or right top element, e.g. we start from left bottom element, if the target > this element, we can prune all of this column's elements and go to next column. So the total time complexity is O(m+n).

	public boolean searchMatrix(int[][] matrix, int target) {
	        int m = matrix.length;
	        if(m == 0) return false;
	        int n = matrix[0].length;
	        int row = m - 1;
	        int col = 0;
	        while(row >= 0 && col < n)
	        {
	            if(matrix[row][col] == target)
	                return true;
	            else if(matrix[row][col] < target)
	            {
	                col++;
	            }else
	            {
	                row--;
	            }
	        }
	        return false;
	    }
