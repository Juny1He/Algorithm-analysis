# Longest Substring Without Repeating Characters

## Question :

Given a string, find the length of the longest substring without repeating characters.

Example 1:

	Input: "abcabcbb"
	Output: 3 
	Explanation: The answer is "abc", with the length of 3. 

Example 2:

	Input: "bbbbb"
	Output: 1
	Explanation: The answer is "b", with the length of 1.

Example 3:

	Input: "pwwkew"
	Output: 3
	Explanation: The answer is "wke", with the length of 3. 
	             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.


## Solution : 

There are three solutions about this questions : 1. Brute Force 2. Sliding Widow with 2 pass. 3. Optimized Sliding Window with 1 pass. 


1. For the Brute Force, we just need to check whether the each substring(i,j) is a valid substring without repeating characters. Thus the total time complexity is O(n^3);

2. For the sliding window with 2 pass, we need two pointers left and right represented as the sliding window's left boundary and right boundary. and use a hash table or array to record the number of character, if there is no duplicate character in the sliding window, we increase the right boundary, otherwise increase the left boundary one by one to decrease the number of character, until there are no duplicate characters.

		public int lengthOfLongestSubstring(String s) {
	        int[] let = new int[256];
	        int left = 0;
	        int ret = 0;
	        for(int i = 0; i < s.length(); i ++)
	        {
	            char ch = s.charAt(i);
	            let[ch]++;
	            while(let[ch] > 1)
	            {
	                let[s.charAt(left++)]--;
	            }
	            ret = Math.max(ret,i-left+1);
	        }
	        	return ret;
	    }

3. For the sliding window with 1 pass, we can use a array or hashmap to record the index of current character(or index plus one). When we meet the duplicate character while traversing, we update the left boundary to the previous same character's index plus one, then update this character's index into the array or hashmap(No need to update the left boundary one by one).

		public int lengthOfLongestSubstring(String s) {
	        if(s.length() == 1) return 1;
	        int[] hash = new int[128];
	        int ret = 0;
	        int start = 0;
	        for(int i = 0; i <s.length(); i ++)
	        {
	            char ch = s.charAt(i);
	            if(hash[ch] != 0)
	            {
	                start = Math.max(hash[ch],start);
	            }
	            ret = Math.max(ret,i - start + 1);
	            hash[ch] = i+1;
	        }
	        return ret;
	    }
