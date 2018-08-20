# Binary Tree Zigzag Level Order Traversal

> Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

按层级遍历二叉树，并以拉链序排列（从左到右，从右到左的顺序交叉进行）

## 解题思路

与102题类似，不过这里要求拉链序。

观察可发现，奇数层 ->，偶数层 <-

我们依然使用队列来保存某一层级的节点，并且依然按从左到右排列。
只不过利用双端队列，
当在奇数层时，从左端弹出，从右端插入。
当在偶数层时，从右端弹出，从左端插入。

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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> list = new ArrayList<>();
        Deque<TreeNode> queue = new LinkedList<>();

        if (root == null) return list;

        queue.offer(root);

        int level = 0;

        while (!queue.isEmpty()) {
            level++;

            int len = queue.size();
            List<Integer> innerList = new ArrayList<>();

            for (int i = 0; i < len; i++) {
                if (level % 2 == 1) {
                    TreeNode peek = queue.poll();
                    innerList.add(peek.val);

                    if (peek.left != null) {
                        queue.offer(peek.left);
                    }
                    if (peek.right != null) {
                        queue.offer(peek.right);
                    }
                } else {
                    TreeNode tail = queue.pollLast();
                    innerList.add(tail.val);

                    if (tail.right != null) {
                        queue.offerFirst(tail.right);
                    }
                    if (tail.left != null) {
                        queue.offerFirst(tail.left);
                    }
                }
            }
            list.add(innerList);
        }

        return list;
    }
}
```

