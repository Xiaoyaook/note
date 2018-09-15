# Permutations

> Given a collection of distinct integers, return all possible permutations.

给一串数组，求全排列

## 解题思路

递归求解，`l.remove(l.size() - 1);`注意这一句很关键，在递归后要把当前最后一个元素删去，不要影响下一个循环。

```Java
class Solution {
    private List<List<Integer>> list = new ArrayList<>();
    
    public List<List<Integer>> permute(int[] nums) {
        
        recur(nums, nums.length, new ArrayList<Integer>());
        
        return this.list;
    }
    
    public void recur(int[] nums, int n, List<Integer> l) {
        if (n == 0) {
            this.list.add(new ArrayList<Integer>(l));
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (l.contains(nums[i])) {
                continue;
            } else {
                l.add(nums[i]);
                recur(nums, n - 1, l);
                l.remove(l.size() - 1);
            }
        }
    }
}
```

Python解法就更简单了，直接调用库函数

```Python
import itertools

class Solution(object):
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        perms = itertools.permutations(nums)
        permlist = [perm for perm in perms]
        return permlist
```