### Reverse Linked List

> Reverse a singly linked list.

反转单链表

```Java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

Java迭代解法
```Java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode frontHead = null;
        while (head != null) {
            ListNode next = head.next;
            head.next = frontHead;
            frontHead = head;
            head = next;
        }
        return frontHead;
    }
}
```

Java递归解法
```Java
class Solution {
    public ListNode reverseList(ListNode head) {
        return reverseListInt(head, null);
    }

    private ListNode reverseListInt(ListNode head, ListNode frontHead) {
        if (head == null) {
            return frontHead;
        }
        ListNode next = head.next;
        head.next = frontHead;
        return reverseListInt(next, head);
    }
}
```

```Python
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None
```

Python迭代解法
```Python
class Solution:
    def reverseList(self, head):
        prev = None
        while head:
            curr = head
            head = head.next
            curr.next = prev
            prev = curr
        return prev
```

Python递归解法
```Python
class Solution:
    def reverseList(self, head):
        return self._reverse(head)

    def _reverse(self, node, prev=None):
        if not node:
            return prev
        n = node.next
        node.next = prev
        return self._reverse(n, node)
```