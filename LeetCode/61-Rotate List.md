### Rotate List

> Given a list, rotate the list to the right by k places, where k is non-negative.

旋转链表,把链表右边的k个元素,旋转到左边.

解题思路:
* 经典的快慢指针解法
* 首先判断没有节点和只有一个节点的情况
* 然后计算rotateTimes
* 然后就到快慢指针表演的时候了,over

```Java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null) {
            return null;
        }
        if (head.next == null) {
            return head;
        }
        
        ListNode pointer = head;
        int length = 1;
        
        while (pointer.next != null) {
            pointer = pointer.next;
            length += 1;
        }
            
        int rotateTimes = k % length;
        
        if (k == 0 || rotateTimes == 0) {
            return head;
        }
        
        ListNode fastPointer = head;
        ListNode slowPointer = head;
        
        for (int i=1;i<=rotateTimes;i++) {
            fastPointer = fastPointer.next;
        }
        
        while (fastPointer.next != null) {
            fastPointer = fastPointer.next;
            slowPointer = slowPointer.next;
        }
        
        ListNode temp = slowPointer.next;
        fastPointer.next = head;
        slowPointer.next = null;
        head = temp;
        
        return head;
        
    }
}
```

```Python
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution(object):
    def rotateRight(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        if not head:
            return None
        if not head.next:
            return head

        pointer = head
        length = 1

        while pointer.next:
            pointer = pointer.next
            length += 1

        rotateTimes = k % length

        if k == 0 or rotateTimes == 0:
            return head

        fastPointer = head
        slowPointer = head

        for a in range(rotateTimes):
            fastPointer = fastPointer.next
        
        while fastPointer.next:
            fastPointer = fastPointer.next
            slowPointer = slowPointer.next

        temp = slowPointer.next
        slowPointer.next = None
        fastPointer.next = head
        head = temp

        return head
```