# Sort List

> Sort a linked list in O(n log n) time using constant space complexity.

对一个单链表进行排序，要求时间复杂度为O(n log n)，空间复杂度为1.

## 解题思路

运用归并排序，或者快速排序。

需要注意的是单链表和数组有些差别，进行快速排序时，无法从后向前遍历。

### 归并排序

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
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) return head;

        // step 1. cut the list to two halves
        ListNode prev = null, slow = head, fast = head;

        while (fast != null && fast.next != null) {
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }

        prev.next = null;

        // step 2. sort each half
        ListNode l1 = sortList(head);
        ListNode l2 = sortList(slow);

        // step 3. merge l1 and l2
        return merge(l1, l2);
    }

    ListNode merge(ListNode l1, ListNode l2) {
        ListNode l = new ListNode(0), p = l;

        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
              p.next = l1;
              l1 = l1.next;
            } else {
                p.next = l2;
                l2 = l2.next;
            }   
            p = p.next;
        }

        if (l1 != null) p.next = l1;

        if (l2 != null) p.next = l2;

        return l.next;
    }
}
```

### 快速排序

> 对于一个链表，以head节点的值作为key，然后遍历之后的节点，可以得到一个小于key的链表和大于等于key的链表；由此递归可以对两个链表分别进行快速。这里用到了快速排序的思想即经过一趟排序能够将小于key的元素放在一边，将大于等于key的元素放在另一边。

快排需要注意的是，我们并不移动结点，而是交换结点值。

> leetcode的测试数据，有一组[2,2,3,3,1,3,2,1…]的数据，只有1-3三个整数，出现65536次。快排面对这组数据，复杂度会降为O(N^2),导致TLE。（如果使用了剪枝技巧，当子数组中的元素全部相等，即最大值等于最小值，就不继续递归。）

```Java
//快速排序
public static void quickSort(ListNode begin, ListNode end){
    if(begin == null || begin == end)
        return;
    
    ListNode index = paration(begin, end);
    quickSort(begin, index);
    quickSort(index.next, end);
}

/**
    * 划分函数，以头结点值为基准元素进行划分
    * @param begin
    * @param end
    * @return
    */
public static ListNode paration(ListNode begin, ListNode end){
    if(begin == null || begin == end)
        return begin;
    
    int val = begin.val;  //基准元素
    ListNode index = begin, cur = begin.next;
    
    while(cur != end){
        if(cur.val < val){  //交换
            index = index.next;
            int tmp = cur.val;
            cur.val = index.val;
            index.val = tmp;
        }
        cur = cur.next;
    }
    
    
    begin.val = index.val;
    index.val = val;
    
    return index;
}
```
---
[单链表排序 Sort List](https://devhui.com/2015/02/23/sort-list/)