### Symmetric Tree

> Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

判断一棵树是否是对称的

解题思路:
* 关键是同时传入的两个参数都是最初的根节点
* 然后让其左右子树比较

```Java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root==null) return true;
        return isMirror(root.left,root.right);
    }

    public boolean isMirror(TreeNode p, TreeNode q) {
    if (p==null && q==null) return true;
    if (p==null || q==null) return false;
    return (p.val==q.val) && isMirror(p.left,q.right) && isMirror(p.right,q.left);
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
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        def isSym(L, R):
            if not L and not R: return True

            if L and R and L.val == R.val:
                return isSym(L.left, R.right) and isSym(L.right, R.left)
            
            return False
        
        return isSym(root, root)
```