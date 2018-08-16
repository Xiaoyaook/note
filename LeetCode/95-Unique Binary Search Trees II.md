# Unique Binary Search Trees II

> Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1 ... n.

和96题类似，不过这里要列出所有类型二查搜索树的根节点。

## 解题思路

我们可以注意到，由 1...n 所构成的所有二查搜索树，中序遍历都是1 -> n。

那么取i为根节点，左子树即为1..i-1，右子树为i+1...n。向下递归，并将左右子树与根节点连接。

然后子树继续向下递归

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<TreeNode> generateTrees(int n) {
        if (n <= 0) return new ArrayList<TreeNode>();
        return genTrees(1,n);
    }
        
    public List<TreeNode> genTrees (int start, int end)
    {

        List<TreeNode> list = new ArrayList<TreeNode>();

        if(start>end)
        {
            list.add(null);
            return list;
        }
        
        if(start == end){
            list.add(new TreeNode(start));
            return list;
        }
        
        List<TreeNode> left,right;
        // 对应1..n每个数字当root的不同情况
        for(int i=start;i<=end;i++)
        {
            
            left = genTrees(start, i-1);
            right = genTrees(i+1,end);
            // 两个for循环嵌套，相当于左子树的种类×右子树的种类
            for(TreeNode lnode: left)
            {
                for(TreeNode rnode: right)
                {
                    TreeNode root = new TreeNode(i);
                    root.left = lnode;
                    root.right = rnode;
                    list.add(root);
                }
            }
                
        }
        
        return list;
    }
}
```