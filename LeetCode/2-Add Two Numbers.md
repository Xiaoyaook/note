### 2. Add Two Numbers

> You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

> You may assume the two numbers do not contain any leading zero, except the number 0 itself.

解题关键是:声明一个变量carry携带进位信息,另外初始化一个头结点.循环直到l1,l2为空且进位为0.

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution { 
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode root = new ListNode(0);
        ListNode n = root;
        int carry = 0;
        while (l1 != null || l2 != null || carry != 0) {
            int v1 = 0,v2 = 0;
            if (l1 != null) {
                v1 = l1.val;
                l1 = l1.next;
            }
            if (l2 != null) {
                v2 = l2.val;
                l2 = l2.next;
            }
            int val = (v1+v2+carry) % 10;
            carry = (v1 + v2 + carry) / 10;
            n.next = new ListNode(val);
            n = n.next;
        }
        return root.next;
    }
}
```

```Python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def addTwoNumbers(self, l1, l2):
        carry = 0
        root = n = ListNode(0)
        while l1 or l2 or carry:
            v1 = v2 = 0
            if l1:
                v1 = l1.val
                l1 = l1.next
            if l2:
                v2 = l2.val
                l2 = l2.next
            carry, val = divmod(v1+v2+carry, 10)
            # divmod(a,b)方法返回的是a//b（除法取整）以及a对b的余数
            # 返回结果类型为tuple
            n.next = ListNode(val)
            n = n.next
        return root.next
```