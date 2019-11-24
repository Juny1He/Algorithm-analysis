# Best Meeting Point

## Question : 

A group of two or more people wants to meet and minimize the total travel distance. You are given a 2D grid of values 0 or 1, where each 1 marks the home of someone in the group. The distance is calculated using Manhattan Distance, where distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|.

Example:

	Input: 

	1 - 0 - 0 - 0 - 1
	|   |   |   |   |
	0 - 0 - 0 - 0 - 0
	|   |   |   |   |
	0 - 0 - 1 - 0 - 0

	Output: 6 

	Explanation: Given three people living at (0,0), (0,4), and (2,2):
	             The point (0,2) is an ideal meeting point, as the total travel distance 
	             of 2+2+2=6 is minimal. So return 6.

## Solution : 

The Brute Force of this problem is compute all the points to people's Manhattan Distance, and find the minimum one. This takes(m^2n^2)

For this problem, we can think it in 1 dimension firstly. If there are a lot of points on a 1 dimension array, which point we choose can get a smallest distance. Take the following 1 dimensional array as example : 


	      median                  mean
	1 - 1 - 1 - 1 - 1 - 0 - 0 - 0 - 0 - 0 - 0 - 0 - 0 - 0 - 0 - 0 - 1

Obviously, we can not choose the mean point as the best point, since when we move choose the left one of the mean point, we can find that the distance decrease 5 , but only increase 1 (there are five points in the left of mean point, but only 1 point in the right of mean). So from this observation, we should the choose median point.  As for two dimensional array, we just choose a point which row and column are both median is OK. 

One way to solve this is add row and column of all the people's points respectively, add sort these two lists, and choose the median point. 

Another more clever solution is just add row and column in a specific order, so we do not need to sort. Consider the one dimensional array again, we just need to add elements index one by one, we can know that the median point is at the median, no need to sort. The total time complexity is O(mn + k) = O(mn) k is the number of people's points. 

	public int minTotalDistance(int[][] grid) {
        int m = grid.length;
        if(m == 0) return 0;
        int n = grid[0].length;
        List<Integer> R = new ArrayList<>();
        List<Integer> C = new ArrayList<>();
        
        for(int i = 0; i < m; i ++)
        {
            for(int j = 0; j < n; j ++)
            {
                if(grid[i][j] == 1)
                R.add(i);
            }
        }
        for(int j = 0; j < n; j ++)
        {
            for(int i = 0; i < m; i ++)
            {
                if(grid[i][j] == 1)
                C.add(j);
            }
        }
        
        int row = R.get(R.size()/2);
        int col = C.get(C.size()/2);
        int dis = 0;
        for(int r : R)
        {
            dis+= Math.abs(r-row);
        }
        
        for(int c : C)
        {
            dis+= Math.abs(c-col);
        }
        return dis;
    }