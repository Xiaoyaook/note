### Construct Binary Tree from Inorder and Postorder Traversal

>Given inorder and postorder traversal of a tree, construct the binary tree.
You may assume that duplicates do not exist in the tree.

由中序和后序序列构造二叉树.

解题思路:
* 与105类似,不过注意,后序排列的最后一个元素才是根节点
* 之后的左子树和右子树的根节点位置需要注意

```Java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

class Solution {
    public TreeNode buildTree(int[] inorder, int[ postorder]) {
        return build(inorder, inorder.length-1, 0, postorder, postorder.length-1);
    }

    public TreeNode build(int[] inorder, int inStart, int inEnd, int[] postorder, int postStart) {
        if (inEnd > inStart) {
            return null;
        }

        TreeNode root = new TreeNode(postorder[postStart]);
        if (inEnd == inStart) {
            return root;
        }
        int index = 0;  // index 是root在当前inorder序列中的位置
        for (int i = inStart; i >= inEnd; i--) {
            if (inorder[i] == root.val) {
                index = i;
                break;
            }
        }
        root.right = build(inorder, inStart, index+1,   postorder, postStart-1);
        root.left = build(inorder, index-1, inEnd,postorder, postStart-(inStart-index)-1);

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
    def buildTree(self, inorder, postorder):
        """
        :type inorder: List[int]
        :type postorder: List[int]
        :rtype: TreeNode
        """
        if inorder:
            ind = inorder.index(postorder.pop())
            root = TreeNode(inorder[ind])
            root.right = self.buildTree(inorder[ind+1:],postorder)
            root.left = self.buildTree(inorder[:ind], postorder)
            
            return root
```