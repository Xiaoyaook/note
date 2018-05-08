### Reverse Linked List II

> Reverse a linked list from position m to n. Do it in-place and in one-pass.

给一个链表,把m到n之间的元素反转.

解题思路:
* 创建dummy node
* 两处循环,一次到m,一次m到n
* m到n的时进行反转


```Python
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution(object):
    def reverseBetween(self, head, m, n):
        """
        :type head: ListNode
        :type m: int
        :type n: int
        :rtype: ListNode
        """
        if head == None or head.next == None:
            return head
        dummyhead = ListNode(0)
        dummyhead.next = head

        p = dummyhead

        for i in range(m-1):
            p = p.next

        curr = p.next
        for i in range(n-m):
            tmp = curr.next
            curr.next = tmp.next
            tmp.next = p.next
            p.next = tmp

        return dummyhead.next
```