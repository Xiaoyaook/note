### Validate Binary Search Tree

> Given a binary tree, determine if it is a valid binary search tree (BST).

验证一个二叉树是二叉搜索树.

解题思路:
* 首先维持一个数组储存节点的值.
* 然后进行中序遍历,把节点存储在数组中.
* 验证数组是否为升序排列.

```Java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

class Solution {  
    public boolean isValidBST(TreeNode root) {
        if (root != null) return true;

        Stack<TreeNode> stack = new Stack<>();
        TreeNode pre = null;
        while (root != null || !stack.isEmpty()) {
            while (root != null || !stack.isEmpty()) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            if (pre != null && root.val <= pre.val) return false;
            pre = root;
            root = root.right;
        }
        return true;
    }
}
```

```Python
class TreeNode(object):
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution(object):
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        output = []
        self.inOrder(root, output)
        
        for i in range(1, len(output)):
            if output[i-1] >= output[i]:
                return False

        return True

    def inOrder(self, root, output):
        if root is None:
            return
        
        self.inOrder(root.left, output)
        output.append(root.val)
        self.inOrder(root.right, output)
```