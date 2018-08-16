# Unique Binary Search Trees

> Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?

给一个n，能构成多少种二叉搜索树（结点由1...n构成）

## 解题思路

动态规划

构建一维数组dp[k]，dp[k] 就代表n为k时，可构成二叉搜索树的种数。

首先我们可以容易的得到dp[1] = 1, dp[2] = 2, dp[3] = 5，并且我们令dp[0] = 1

下面给一个例子，假如我们要求{1,2,3,4,5}构成二叉搜索树的种数：
* 首先以 1 为根节点，它的左子树为空，右子树{2,3,4,5}。可构成的种数为 dp[0] * dp[4]，这样就把问题化简了，可以看出后一个状态，依赖之前的状态。
* 同样的，以 2 为节点，左子树{1}，右子树{3,4,5}。可构成的种数为 dp[1] * dp[3]
* 以 3 为节点， dp[2] * dp[2]
* 以 4 为节点， dp[3] * dp[1]
* 以 5 为节点， dp[4] * dp[0]

接着把五种情况的结果相加，即为dp[5]

```Java
class Solution {
    public int numTrees(int n) {
        int [] dp = new int[n+1];
        dp[0]= 1;
        dp[1] = 1;
        for(int level = 2; level <=n; level++)
            for(int root = 1; root<=level; root++)
                dp[level] += dp[level-root]*dp[root-1];
        return dp[n];
    }
}
```


