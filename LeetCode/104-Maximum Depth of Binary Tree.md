### Maximum Depth of Binary Tree

> Given a binary tree, find its maximum depth.
The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

找出二叉树的最大深度

解题思路:
* 递归调用
* 若当前节点为null则返回0,否则返回左右子树深度最大的那个并加1

```Java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

class Solution {
    public int maxDepth(TreeNode root) {
        return root==null? 0 : Math.max(maxDepth(root.left), maxDepth(root.right))+1;
    }
}
```

```Python
class TreeNode(object):  # 构造二叉树节点
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution(object):
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """  
        if root is None:  
            return 0  
        else:  
            return max(self.maxDepth(root.left),self.maxDepth(root.right))+1 
```