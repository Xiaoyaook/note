# Permutation Sequence

> The set [1,2,3,...,n] contains a total of n! unique permutations.
>
> Given n and k, return the kth permutation sequence.
>
> Given n will be between 1 and 9 inclusive.
Given k will be between 1 and n! inclusive.

全排列问题，按从小到大顺序，求第k个全排列的值。

## 解题思路

n,k已经给了限制范围，故不用考虑特殊情况。

用一个布尔数组判断该数字是否被使用过。

用一个类变量ans来保存最终结果，不用保存全部的全排列。

```Java
class Solution {
    public int count = 0;
    public String ans = "";

    public String getPermutation(int n, int k) {
        boolean[] used = new boolean[n+1];
        dfs(n, k, "", used);
        return this.ans;
    }

    public void dfs(int n, int k, String str, boolean[] used) {
        if (str.length() == n) {
            this.count += 1;
            if (this.count == k) this.ans = str;
        }

        for (int i = 1; i <= n; i++) {
            if (used[i]) continue;

            used[i] = true;
            dfs(n, k, str + i, used);
            used[i] = false;
        }
    }
}
```