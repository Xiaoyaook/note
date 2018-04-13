### Remove Duplicates from Sorted List

> Given a sorted linked list, delete all duplicates such that each element appear only once.

给一个排序后的链表,删除所有重复的元素

解题思路:
* 首先判断head和head.next是否为null
* 因为已经排过序了,所以直接比较相邻的两个节点就好
* 链表删除元素的操作很简单
* 注意循环条件是`p.next != null`

```Java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}

class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode p = head;
        
        while (p.next != null) {
            if (p.val == p.next.val) {
                p.next = p.next.next;
            }
            else {
                p = p.next;
            }
        }
        
        return head;
    }
}
```

```Python
class ListNode(object):
    def __init__(self,x):
        self.val = x
        self.next = None

class Solution(object):
    def deleteDuplicates(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """ 
        if head == None or head.next == None:
            return head
        p = head
        while p.next:
            if p.val == p.next.val:
                p.next = p.next.next
            else:
                p = p.next
        return head
```