# Combinations

> Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

组合问题，给出由k个数字组成的 1 -> n 的组合情况。

## 解题思路

和排列问题类似，不过更加简化了。不需要used数组，递归后也不需要每次都从头遍历，从start处开始即可。

```Java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> list = new ArrayList<>();
        backtracking(list, new ArrayList<>(), 1, n, k);
        return list;
    }
    
    public void backtracking(List<List<Integer>> list, List<Integer> tempList, int start, int end, int limit) {
        if (tempList.size() == limit) { 
            list.add(new ArrayList<>(tempList));
            return;
        }
        for (int i = start; i <= end; i ++) {
        tempList.add(i);
        backtracking(list, tempList, i + 1, end, limit);
        tempList.remove(tempList.size() - 1);
        }
    }
}
```