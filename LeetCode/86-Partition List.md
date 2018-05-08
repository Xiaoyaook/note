### Partition List

> Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

> You should preserve the original relative order of the nodes in each of the two partitions.

给一个链表和一个值x,把链表分为两部分: 小于x和大于等于x,前者在前后者在后,并保证其相对位置不变.

解题思路:
* 思路就是直接分别创建两个链表,最后再把这两个链表连接
* 创建两个空节点,并分别有两个引用指向他们
* 以x为判断条件,去给两个链表增加值
* 最后连接

```Java
```

```Python
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution(object):
    def partition(self, head, x):
        """
        :type head: ListNode
        :type x: int
        :rtype: ListNode
        """
        h1 = l1 = ListNode(0)
        h2 = l2 = ListNode(0)
        while head:
            if head.val < x:
                l1.next = head
                l1 = l1.next
            else:
                l2.next = head
                l2 = l2.next
            head = head.next

        l2.next = None
        l1.next = h2.next

        return h1.next
```