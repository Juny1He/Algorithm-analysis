# My Calendar II

## Question : 

Implement a MyCalendarTwo class to store your events. A new event can be added if adding the event will not cause a triple booking.

Your class will have one method, book(int start, int end). Formally, this represents a booking on the half open interval [start, end), the range of real numbers x such that start <= x < end.

A triple booking happens when three events have some non-empty intersection (ie., there is some time that is common to all 3 events.)

For each call to the method MyCalendar.book, return true if the event can be added to the calendar successfully without causing a triple booking. Otherwise, return false and do not add the event to the calendar.

Your class will be called like this: MyCalendar cal = new MyCalendar(); MyCalendar.book(start, end)
Example 1:

	MyCalendar();
	MyCalendar.book(10, 20); // returns true
	MyCalendar.book(50, 60); // returns true
	MyCalendar.book(10, 40); // returns true
	MyCalendar.book(5, 15); // returns false
	MyCalendar.book(5, 10); // returns true
	MyCalendar.book(25, 55); // returns true
	Explanation: 
	The first two events can be booked.  The third event can be double booked.
	The fourth event (5, 15) can't be booked, because it would result in a triple booking.
	The fifth event (5, 10) can be booked, as it does not use time 10 which is already double booked.
	The sixth event (25, 55) can be booked, as the time in [25, 40) will be double booked with the third event;
	the time [40, 50) will be single booked, and the time [50, 55) will be double booked with the second event.
 

Note:

1. The number of calls to MyCalendar.book per test case will be at most 1000.
2. In calls to MyCalendar.book(start, end), start and end are integers in the range [0, 10^9].

## Solution : 

The Brute Force solution is use two list, one is to record the event that be added, one is to record that time interval that might be colided with.

One thing need to note that there are two ways to implement this solution, one is traverse the calendar list, if the current has already cause double booking, then we check wether it might cause triple booking in overlap list. Another way is traverse the overlap list first to check wether does it cause triple booking, then traverse the calendar list to update the calendar list. 

The following code is the second way solution (Time Complexity is O(n^2)):

	class MyCalendarTwo {
	    List<int[]> overlap = new ArrayList<>();
	    List<int[]> calendar = new ArrayList<>();
	    public MyCalendarTwo() {
	        
	    }
	    
	    public boolean book(int start, int end) {
	        for(int i = 0; i < overlap.size(); i ++)
	        {
	            int[] temp = overlap.get(i);
	            int temp_start = temp[0];
	            int temp_end = temp[1];
	            if(start >= temp_end || end <= temp_start) continue;
	            return false;
	        }
	        for(int i = 0; i < calendar.size(); i ++)
	        {
	            int[] temp = calendar.get(i);
	            int temp_start = temp[0];
	            int temp_end = temp[1];
	            if(start >= temp_end || end <= temp_start) continue;
	            overlap.add(new int[]{Math.max(temp_start,start),Math.min(temp_end,end)});
	        }
	        calendar.add(new int[]{start,end});
	        return true;
	    }
	}



Another solution is like use a scan line to scan the event interval iteratively, and calculate in current time how many event occur, if the number is greater than 3, return false.

	class MyCalendarTwo {
	    TreeMap<Integer,Integer> map;
	    public MyCalendarTwo() {
	        map = new TreeMap<>();
	    }
	    
	    public boolean book(int start, int end) {
	        map.put(start,map.getOrDefault(start,0)+1);
	        map.put(end,map.getOrDefault(end,0)-1);
	        int active = 0;
	        for(int d : map.values())
	        {
	            active += d;
	            if(active >= 3)
	            {
	                map.put(start,map.get(start) - 1);
	                map.put(end,map.get(end)+1);
	                if(map.get(start) == 0)
	                    map.remove(start);
	                return false;
	            }
	        }
	        return true;
	    }
	}


