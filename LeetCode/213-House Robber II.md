# House Robber II

> You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
>
> Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

198题的延伸版，现在数组首尾相连，抢了第一家就不能抢最后一家。

通过198题，我们已经能得到对于一列元素所能求得的最大和。那么对于本题首尾相连的情况，我们只需要将环给打破，就可以利用198题的结果。

这里我们进行假设，当打劫了第一家时，我们就不再考虑最后一家。当打劫最后一家时，我们就不考虑第一家。这样就把环打破了，然后我们取两种情况下，数值大的那个。（这里考虑最简单的情况，并非限定于第一家和最后一家，任意相邻的两家都是可以的）

我们需要把198题的答案修改一下，多接收两个参数，分别代表在数组中的起始位置和终止位置。

```Java
    private int rob(int[] num, int lo, int hi) {
        int include = 0, exclude = 0;
        for (int j = lo; j <= hi; j++) {
            int i = include, e = exclude;
            include = e + num[j];
            exclude = Math.max(e, i);
        }
        return Math.max(include, exclude);
    }
```

本题Java实现：
```Java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        return Math.max(rob(nums, 0, nums.length - 2), rob(nums, 1, nums.length - 1));
    }
    
    private int rob(int[] num, int lo, int hi) {
        int include = 0, exclude = 0;
        for (int j = lo; j <= hi; j++) {
            int i = include, e = exclude;
            include = e + num[j];
            exclude = Math.max(e, i);
        }
        return Math.max(include, exclude);
    }
}
```

