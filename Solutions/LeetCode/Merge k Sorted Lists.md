### Algorithm

1. Create a `PriorityQueue` (a minHeap)
1. Insert 1st `ListNode` of each list into minHeap
1. Loop on removing minimum from minHeap and putting into solution list. For every removal, refer back to the list that the ListNode came from to add the next ListNode into minHeap.

### Provided code

```java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

### Solution

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        // checking size since PriorityQueue cannot have initial size of 0.
        if (lists == null || lists.length == 0) {
            return null;
        }

        Queue<ListNode> minHeap = new PriorityQueue<ListNode>(lists.length, new ListNodeComparator());

        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        for (ListNode node : lists) {
            if (node != null) {
                minHeap.add(node);
            }
        }

        while (!minHeap.isEmpty()) {
            ListNode node = minHeap.remove();
            tail.next = node;
            tail = tail.next;
            if (node.next != null) {
                minHeap.add(node.next);
            }
        }

        return dummy.next;
    }
}

class ListNodeComparator implements Comparator<ListNode> {
    @Override
    public int compare(ListNode node1, ListNode node2) {
        return Integer.compare(node1.val, node2.val);
    }
}
```

### Time/Space Complexity

- let `n` = length of longest list
- let `k` = number of lists
- Time Complexity: `O(nk log k)` if list lengths are equal. Otherwise, replace `nk` with number of items total in all the lists.
- Space Complexity: `O(k)`, as that's the max size of our PriorityQueue.
