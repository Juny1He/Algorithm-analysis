# Minimum Window Substring

## Question :


Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

Example:

    Input: S = "ADOBECODEBANC", T = "ABC"
    Output: "BANC"
Note:

1. If there is no such window in S that covers all characters in T, return the empty string "".
2. If there is such window, you are guaranteed that there will always be only one unique minimum window in S.



## Solution : 

For this problem, the Brute Force Solution is find check all of the substring(i,j), and find the minimum length substring which can satisfy the requirement. Find all of the substring takes O(n^2) time, check the substring takes O(n) time, thus the total time complexity is O(n^3). 

However, we spent too much time on check whether a substring is a valid substring. We can use two pointers to solve this problem. We construct a sliding window, and if the sliding window can not cover all the characters in t, we continually move right boundary of the sliding window, until the substring in sliding window satisfy the requirement. Then, we can move the left boundary, until the substring does not meet the requirement, then the minimum length might be the substring before we moved the left boundary. Then, we keep moving the right boundary, until it goes to end. 

The total time complexity is O(n), space complexity is O(n);

    public String minWindow(String s, String t) {
    //  Use a array to store the characters in t.
        HashSet<Character> set = new HashSet();
        int[] let = new int[256];
        for(int i = 0; i < t.length(); i ++)
        {
            let[t.charAt(i)]++;
            set.add(t.charAt(i));
        }
        int num = 0;
        int left = 0;
        int l = 0;
        int r = 0;
        int min_len = Integer.MAX_VALUE;
        for(int i = 0; i < s.length(); i ++)
        {
            char ch = s.charAt(i);
            if(set.contains(ch))
            {
                let[ch]--;
                if(let[ch] >= 0)
                {
                    num++;
                }
            }
            if(num == t.length())
            {
                while(num == t.length())
                {
                    if(set.contains(s.charAt(left)))
                    {
                        let[s.charAt(left)]++;
                        if(let[s.charAt(left)] > 0)
                        {
                            num--;
                        }
                    }
                    left++;
                }
                if(i - left +1 < min_len)
                {
                    r = i;
                    l = left-1;
                    min_len = r - l + 1;
                }
            }
        }
        return min_len == Integer.MAX_VALUE ? "" : s.substring(l,r+1);
     }
    }