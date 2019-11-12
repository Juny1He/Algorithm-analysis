# Sort List

## Question

Sort a linked list in O(n log n) time using constant space complexity.

Example 1:

	Input: 4->2->1->3
	Output: 1->2->3->4
Example 2:

	Input: -1->5->3->4->0
	Output: -1->0->3->4->5

## Solution:

For this question, there are three solution : (1) Insertion sort (2) Merge sort with recursion (3) Merge sort with iteration.

(1) Insertion sort :

For example, 4 -> 2 -> 1 -> 3, firstly we set pivot as 4, then find the first smallest element no smaller than 4 before 4 (4 itself), then insert it before 4 (actually, there is no change). Secondely, set the pivot as 2, similarly, find the first smallest element no smaller than 2 before 2 (it is 4), then the list becomes 2 -> 4 -> 1 -> 3. Then, set the pivot as the third element 1, find the smallest element no smaller than 1 (it is 2), insert the 1 before 2, the sequence becomes 1 -> 2 -> 4 -> 3. Finally, set the pivot at the forth element 3, find the smallest element no smaller than 3 before 3 (it is 4), insert 3 before 4. The list becomes 1 -> 2 -> 3 -> 4.

The time complexity is O(n^2) and space complexity is O(1).

	public static ListNode sortList(ListNode head) {
	        if(head == null || head.next == null){
	            return head;
	        }
        ListNode dummy = new ListNode(0);
        ListNode prev = dummy;
        ListNode curr = head;
        ListNode next = null;
        while(curr != null){
            next = curr.next;
            while(prev.next != null && prev.next.val < curr.val){
                prev = prev.next;
            }
            curr.next = prev.next;
            prev.next = curr;
            curr = next;
            prev = dummy;
        }
        return dummy.next;
    }

One thing need to note about the code that there should be a variable next to record the next pivot we should to set.


(2) Merge sort with recursion

Basically, merge sort is based on divide & conquer, just split list into to part, and sort them respectively then merge then together. The time complexity is O(nlogn), space complexity is O(logn).

	public ListNode sortList(ListNode head) {
        if(head == null || head.next == null) return head;
        ListNode fast = head;
        ListNode slow = head;
        ListNode pre = null;
        while(fast != null && fast.next != null)
        {
            pre = slow;
            fast = fast.next.next;
            slow = slow.next;
        }
        pre.next = null;
        ListNode left = sortList(head);
        ListNode right = sortList(slow);    
        ListNode ret = merge(left,right);
        return ret;
    }
    public ListNode merge(ListNode l1, ListNode l2)
    {
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        while(l1 != null && l2 != null)
        {
            if(l1.val < l2.val)
            {
                cur.next = l1;
                l1 = l1.next;
            }else
            {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        
        if(l1 == null)
        {
            cur.next = l2;
        }else
        {
            cur.next = l1;
        }
        return dummy.next;
    }

One thing need to note about this code is that when we split the original list into two, we use fast and slow pointers. Since it is recursion, we should add a if statement to check when the head or head.next is null. 

(3) Merge sort with iteration

This version of merge sort is use a step veriable to split the list into size of 2^i. So the space complexity becomes O(1);

	public ListNode sortList(ListNode head) {
        if(head == null || head.next == null) return head;
        ListNode dummy = new ListNode(-1);   
        dummy.next = head;
        ListNode pre = dummy;
        ListNode next = dummy;
        int len = 0;
        while(head != null)
        {
            head = head.next;
            len++;
        }
        
        for(int i = 1; i < len; i = i*2)
        {
            ListNode prev = dummy;
            ListNode cur = dummy.next;
            while(cur != null)
            {
                ListNode left = cur;
                ListNode right = split(left,i);
                cur = split(right,i);
                prev = merge(left,right,prev);
            }
        }
        return dummy.next;
    }
    public ListNode split(ListNode head, int step)
    {
        if(head == null) return head;
        for(int i = 1; head.next != null && i < step; i ++)
        {
            head = head.next;
        }
        
        ListNode right = head.next;
        head.next = null;
        return right;
    }
    public ListNode merge(ListNode l1, ListNode l2,ListNode prev)
    {
        
        ListNode cur = prev;
        while(l1 != null && l2 != null)
        {
            if(l1.val < l2.val)
            {
                cur.next = l1;
                l1 = l1.next;
            }else
            {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        
        if(l1 == null)
        {
            cur.next = l2;
        }else
        {
            cur.next = l1;
        }
        while(cur.next != null) cur = cur.next;
        return cur;
    }

