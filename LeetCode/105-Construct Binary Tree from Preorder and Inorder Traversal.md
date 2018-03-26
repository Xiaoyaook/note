### Construct Binary Tree from Preorder and Inorder Traversal

> Given preorder and inorder traversal of a tree, construct the binary tree.
You may assume that duplicates do not exist in the tree.

由前序和中序遍历的序列构造二叉树.

解题思路:
* 我们知道preorder的第一个元素是根节点,那么把这个根节点取出来,得出这个根节点在inorder的位置ind.
* 那么ind左边的元素就是树的左叉,右边的元素就是树的右叉.
* 然后递归调用即可.

```Java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return helper(0, 0, inorder.length - 1, preorder, inorder);
    }

    public TreeNode helper(int preStart, int inStart, int inEnd, int[] preorder, int[] inorder) {
        if (preStart > preorder.length - 1 || inStart > inEnd) {
            return null;
        }

        TreeNode root = new TreeNode(preorder[preStart]);
        int inIndex = 0;  // inorder现在的根的序列值
        for (int i = inStart; i <= inEnd; i++) {
            if (inorder[i] == root.val) {
                inIndex = i;
            }
        }
        root.left = helper(preStart + 1, inStart, inIndex - 1, preorder, inorder);
        root.right = helper(preStart + inIndex - inStart + 1, inIndex + 1, inEnd, preorder, inorder);
        return root;
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
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        if inorder:
            ind = inorder.index(preorder.pop(0))  # 中间值(index的值)
            root = TreeNode(inorder[ind])
            root.left = self.buildTree(preorder, inorder[0:ind])
            root.right = self.buildTree(preorder, inorder[ind+1:])
            
            return root
        
```