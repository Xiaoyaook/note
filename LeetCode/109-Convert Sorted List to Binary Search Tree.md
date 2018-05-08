### Convert Sorted List to Binary Search Tree

> Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

给一个以升序排列的单链表,转换其为高度平衡二叉搜索树.

解题思路:
* 快慢指针解法
* 首先判断head与head.next存在与否
* 接着就运用快慢指针,把链表分割为两部分,即左子树,root,右子树
* 然后递归调用

```Python
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None

class TreeNode(object):
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution(object):
    def sortedListToBST(self, head):
        """
        :type head: ListNode
        :rtype: TreeNode
        """
        if not head:
            return
        if not head.next:
            return TreeNode(head.val)
        
        slow, fast = head, head.next.next
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
        
        # tmp points to root
        tmp = slow.next
        # cut down the left child
        slow.next = None

        root = TreeNode(tmp.val)
        root.left = self.sortedListToBST(head)
        root.right = self.sortedListToBST(tmp.next)
        return root
```