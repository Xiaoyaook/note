### Remove Duplicates from Sorted List II

> Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

给一个排序后的链表,删除所有,有重复的元素

解题思路:
* 首先创建一个dummy node
* 创建一个pre 指向dummy node
* 当有两个值相同时,进入第二层循环,把之后的所有重复元素都删去,这样就剩一个被重复的元素了
* 然后再来删去这个被重复的元素,因为在把所有重复元素都删去后,head要比pre快一个元素,所以直接让head再往后移一位,然后`pre.next = head`
* 如果值不相同,则pre和head同时向后移动一位
* 最后返回头结点,即我们创建的dummy的next

```Java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) {val = x; }
}

class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(0);
        ListNode pre = dummy;
        
        dummy.next = head;
        
        while (head != null && head.next != null) {
            if (head.val == head.next.val) {
                while (head != null && head.next != null && head.val == head.next.val) {
                    head = head.next;
                }
                head = head.next;
                pre.next = head;
            }
            else {
                pre = pre.next;
                head = head.next;
            }
        }
        
        return dummy.next;
    }
}
```

```Python
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution(object):
    def deleteDuplicates(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        dummy = pre = ListNode(0)
        dummy.next = head

        while head and head.next:
            if head.val == head.next.val:
                while head and head.next and head.val == head.next.val:
                    head = head.next
                head = head.next
                pre.next = head
            else:
                pre = pre.next
                head = head.next
        
        return dummy.next
```