# 322. Coin Change

> You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

给一些面额的纸币，和一个总额。问由这些纸币凑成总额所需要的最少数量。

容易想到贪心，不过会有情况不符合，如{1,3,4}，6。贪心的话是4,1,1。而最优解是3,3。

所以本题还是动态规划

解题思路：

构建长度为amount + 1的数组，返回值dp[amount]即构成总额的最少数量。
初始化数组，先把数组用-1填充。dp[0]为0。

dp[amount]的状态是由dp[i-c]的状态构建出来的，由此一直往前递推。

```Java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[1 + amount];

        Arrays.fill(dp, -1);
        dp[0] = 0;

        for (int i = 1; i <= amount; ++i) {
            int min = Integer.MAX_VALUE;
            // find the minimum one.
            for(int c: coins) {
                if (i >= c && -1 != dp[i - c]) min = Math.min(min, dp[i - c]);
            }
            if (Integer.MAX_VALUE != min) dp[i] = 1  + min;
        }

        return dp[amount];
    }
}
```
