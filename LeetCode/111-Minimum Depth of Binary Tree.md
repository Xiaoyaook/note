### Minimum Depth of Binary Tree

> Given a binary tree, find its minimum depth.

> The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

找到二叉树的最小深度

解题思路:
* 递归调用minDepth().
* 先判断当前节点为空的情况.
* 若当前节点无左子树有右子树,则只去递归调用右子树,反之亦然.
* 同时有左右子树,则利用Math类的静态方法去选择小的那个.

```Java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        if (root.left == null && root.right != null) {
            return minDepth(root.right) + 1;
        }
        if (root.left != null && root.right == null) {
            return minDepth(root.left) + 1;
        }
        return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
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
    def minDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root == None:
            return 0
        if root.left == None and root.right != None:
            return self.minDepth(root.right) + 1
        if root.left != None and root.right == None:
            return self.minDepth(root.left) + 1
        return min(self.minDepth(root.left), self.minDepth(root.right)) + 1
```