# Permutations II

> Given a collection of numbers that might contain duplicates, return all possible unique permutations.

给一串可重复的数字，求其全排列，不允许有重复。

## 解题思路

跟46题不同的是，这里数字允许重复，故如何判断重复数字的使用情况是个问题。

现在用一个布尔数组 boolean[] used 来表示数组元素的使用情况，
对数组进行排序，可以把相同的元素聚集起来，这一步为下一步做铺垫。
当一个元素与其前一个元素相同时，仅当前一个元素已经使用过的情况下，这个元素才能够被使用（避免了全排列重复问题，省了我们使用set去做判断了）。

最后，注意递归后，要把`used[i]=false; list.remove(list.size()-1);`这两个使用情况重置，避免影响下一个循环。

```Java
public class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(nums==null || nums.length==0) return res;
        boolean[] used = new boolean[nums.length];
        List<Integer> list = new ArrayList<Integer>();
        Arrays.sort(nums);
        dfs(nums, used, list, res);
        return res;
    }

    public void dfs(int[] nums, boolean[] used, List<Integer> list, List<List<Integer>> res){
        if(list.size()==nums.length){
            res.add(new ArrayList<Integer>(list));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(used[i]) continue;
            if(i>0 &&nums[i-1]==nums[i] && !used[i-1]) continue;
            used[i]=true;
            list.add(nums[i]);
            dfs(nums,used,list,res);
            used[i]=false;
            list.remove(list.size()-1);
        }
    }
}
```