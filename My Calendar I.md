# My Calendar I

## Question : 

Implement a MyCalendar class to store your events. A new event can be added if adding the event will not cause a double booking.

Your class will have the method, book(int start, int end). Formally, this represents a booking on the half open interval [start, end), the range of real numbers x such that start <= x < end.

A double booking happens when two events have some non-empty intersection (ie., there is some time that is common to both events.)

For each call to the method MyCalendar.book, return true if the event can be added to the calendar successfully without causing a double booking. Otherwise, return false and do not add the event to the calendar.

Your class will be called like this: MyCalendar cal = new MyCalendar(); MyCalendar.book(start, end)
Example 1:

	MyCalendar();
	MyCalendar.book(10, 20); // returns true
	MyCalendar.book(15, 25); // returns false
	MyCalendar.book(20, 30); // returns true
	Explanation: 
	The first event can be booked.  The second can't because time 15 is already booked by another event.
	The third event can be booked, as the first event takes every time less than 20, but not including 20.
 

Note:

1. The number of calls to MyCalendar.book per test case will be at most 1000.
2. In calls to MyCalendar.book(start, end), start and end are integers in the range [0, 10^9].
 

## Solution :

The Brute Force of this solution is quite straightforward : just use a list to store all the intervals and compare the current interval's start and end with existing intervals' start and end. 

	class MyCalendar {
	    List<List<Integer>> time = new ArrayList<>();
	    public MyCalendar() {

	    }

	    public boolean book(int start, int end) {
		if(time.size() != 0)
		{
		    for(int i = 0; i < time.size(); i ++)
		    {
			List<Integer> temp = time.get(i);
			int temp_start = temp.get(0);
			int temp_end = temp.get(1);
			if(temp_end <= start || temp_start >=end ) continue;
			return false;
		    } 
		}
		List<Integer> temp = Arrays.asList(start,end);
		time.add(temp);
		return true;
	    }

	}

	/**
	 * Your MyCalendar object will be instantiated and called as such:
	 * MyCalendar obj = new MyCalendar();
	 * boolean param_1 = obj.book(start,end);
	 */

A more optimized solution is maintain the order of the list and use a binary search to find the interval most likely colide with the current interval. In Java, we can use TreeMap to implement this solution. 

	class MyCalendar {
	    TreeMap<Integer,Integer> map = new TreeMap<>();
	    public MyCalendar() {
	        
	    }
	    
	    public boolean book(int start, int end) {
	        Integer prev = map.floorKey(start);
	        Integer next = map.ceilingKey(start);
	        if((prev == null || map.get(prev) <= start) &&  (next == null || next >= end))
	        {
	            map.put(start,end);
	            return true;
	        }
	        
	        return false;
	    }
	}
	/**
	 * Your MyCalendar object will be instantiated and called as such:
	 * MyCalendar obj = new MyCalendar();
	 * boolean param_1 = obj.book(start,end);
	 */
