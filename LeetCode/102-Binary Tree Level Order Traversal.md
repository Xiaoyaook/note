# Binary Tree Level Order Traversal

> Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

给一个二叉树，按树的层级，从左到右遍历。

## 解题思路

解题的关键就是用一个队列去保存某一层级元素（非null），先入先出。

用 curL 表示当前层级有效元素的长度。

从队首弹出元素，从队末加入元素。不断循环，直到队列为空。

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
    public List<List<Integer>> levelOrder(TreeNode root) {
       List<List<Integer>> result = new ArrayList<List<Integer>>();
       
       if(root == null){
          return result;
       }
       
       Queue<TreeNode> queue = new LinkedList<TreeNode>();
       queue.offer(root);
       
       int curL = 0;
       while(!queue.isEmpty()){
           List<Integer> levelRs = new ArrayList<Integer>(); 
           curL = queue.size();
           for(int i=0;i<curL;i++){
               TreeNode peek = queue.poll();
               levelRs.add(peek.val);
               if(peek.left!=null){
                   queue.offer(peek.left);
               }
               if(peek.right!=null){
                   queue.offer(peek.right);
               }
           }
           result.add(levelRs);
       }
       
       return result;
    }  
}
```

Python 用列表生成式的方法相当简洁。

```Python
def levelOrder(self, root):
    ans, level = [], [root]
    while root and level:
        ans.append([node.val for node in level])            
        level = [kid for n in level for kid in (n.left, n.right) if kid]
    return ans
```