### Balanced Binary Tree

> Given a binary tree, determine if it is height-balanced.

> a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

判断一个二叉树是否为高度平衡二叉树

解题思路:
* 对每一个节点都去调用maxDpeth
* 如果左右子树高度差大于1则直接`result = false`
* 否则返回`1 + Math.max(l, r)`

```Java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

class Solution {
    private boolean result = true;

    public boolean isBalanced(TreeNode root) {
        maxDepth(root);
        return result;
    }

    public int maxDpeth(TreeNode root) {
        if (root == null) return 0;
        int l = maxDepth(root.left);
        int r = maxDepth(root.right);
        if (Math.abs(l - r) > 1) result = false;
        return 1 + Math.max(l, r);
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
    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return True
        
        return abs(self.getHeight(root.left) -self.getHeight(root.right)) < 2 and self.isBalanced(root.left) and self.isBalanced(root.right)
    
    def getHeight(self, root):
        if not root:
            return 0
        return 1 + max(self.getHeight(root.left), self.getHeight(root.right))
```