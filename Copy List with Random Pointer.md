# Copy List with Random Pointer

## Question 

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.  

Return a deep copy of the list.  

 

Example 1:  

	Input:
	{"$id":"1","next":{"$id":"2","next":null,"random":{"$ref":"2"},"val":2},"random":{"$ref":"2"},"val":1}
	Explanation:
	Node 1's value is 1, both of its next and random pointer points to Node 2.
	Node 2's value is 2, its next pointer points to null and its random pointer points to itself.
 

Note:  

You must return the copy of the given head as a reference to the cloned list.  

## Solution:

1. This problem is a little bit tricky because of the node's random point, when we want to point the new node to its random node, we do not know whether does it be generated before. Thus, a intuitive solution is using a hash map to record the node we have generated. This hash map is used to map the new node to old node. So if we have generated it before, we just map.get(old node) to get the new old. This takes O(n) time, and O(n) space. 

		private HashMap<Node, Node> map = new HashMap<>();
		public Node copyRandomList(Node head) {
			if(head == null) return null;
			Node oldNode = head;
			Node dummy = new Node(-1,null,null);
			Node cur = dummy;
			while(oldNode != null)
			{
			    cur.next = getClonedNode(oldNode);
			    cur.next.random = getClonedNode(oldNode.random);
			    cur = cur.next;
			    oldNode = oldNode.next;
			}
			return dummy.next;
		}	
		public Node getClonedNode(Node node)
		{
			if(node == null) return null;
			if(!map.containsKey(node))
			    map.put(node,new Node(node.val,null,null));
			return map.get(node);
		}

2. Another optimized solution is combining the old node and new node together alternately. Then, connect the new node to its random node, then split the new node and old node. This takes O(n) time, and O(1) time. 

		public Node copyRandomList(Node head) {
			if(head == null) return null;
			Node p = head;
			while(p != null)
			{
			    Node add = new Node(p.val,null,null);
			    add.next = p.next;
			    p.next = add;
			    p = p.next.next;
			}
			p = head;
			while(p!= null)
			{
			    p.next.random = p.random == null ? null : p.random.next;
			    p = p.next.next;
			}
			Node dummy = new Node(-1,null,null);
			Node cur = dummy;
			p = head;
			while(p!= null)
			{
			    cur.next = p.next;
			    p.next= p.next.next;
			    p = p.next;
			    cur = cur.next;
			}
			return dummy.next;
	    }
