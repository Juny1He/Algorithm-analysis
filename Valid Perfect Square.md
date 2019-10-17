# Valid Perfect Square

## Questino  
Given a positive integer num, write a function which returns True if num is a perfect square else False.

Note: Do not use any built-in library function such as sqrt.  

Example 1:  

Input: 16  
Output: true  
Example 2:  
  
Input: 14  
Output: false  

## Solution:

### Solution 1:  

Brute Force : we can check whether i * i is equal to num (0 <= i <= num), if there is a such i equals to num, return true, otherwise return false. This algorithm takes O(n) time, another thing we can note that if i * i > num, we can return false and stop algorithm. 

### Solution 2:

Math Method : For squre number, they have a property is that 1 + 3 + ... + (2n-1) = (2n-1 + 1) * n / 2 = n ^ 2. For example, 1 = 1, 4 = 1 + 3, 9 = 1 + 3 + 5, 16 = 1 + 3 + 5 + 7 + 9 etc. This takes O(sqrt(n)) time.

	public boolean isPerfectSquare(int num) {
	     int i = 1;
	     while (num > 0) {
	         num -= i;
	         i += 2;
	     }
	     return num == 0;
	 }


### Solution 3:

Binary Search:

This question can be formulated as find a number i, i * i == num, from 0 to num. And from 0 to num is in ascending order. So we can use binary search and this takes O(log n) time. 


	public boolean isPerfectSquare(int num) {
        long left = 0;
        long right = num;
        while(left <= right)
        {
            long middle = (left + right >>> 1);
            if(middle * middle == num) return true;
            else if(middle * middle < num)
            {
                left = middle+1;
            }else
            {
                right = middle - 1;
            }
        }
        return false;
    }

Note: For java, the range for int is -2,147,483,648 to 2,147,483, 647, so the middle * middle maybe transcend this range. So we should define the middle as long but not int.
