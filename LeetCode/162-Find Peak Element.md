# Find Peak Element

> A peak element is an element that is greater than its neighbors.
Given an input array nums, where nums[i] ≠ nums[i+1], find a peak element and return its index.
The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.
You may imagine that nums[-1] = nums[n] = -∞.

找到一维数组的峰值。峰值的意思是，大于其左边和右边的元素。有多个峰值的情况下返回一个即可。

## 解题思路

最容易想到的方法应该是顺序查找，注意边界条件

### 顺序查找 O(n)

```Java
class Solution {
    public int findPeakElement(int[] nums) {
        if (nums.length == 1) {
            return 0;
        } 
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] < nums[i-1]) {
                return i - 1;
            }
        }
        return nums.length - 1;
    }
}
```

对顺序查找进行优化，注意数组不要越界
### 二分法 O(lgn)
递归
```Java
class Solution {
    public int findPeakElement(int[] nums) {
        if (nums.length == 1) {
            return 0;
        } 
        return helper(nums, 0, nums.length-1);
    }
    public int helper(int[] nums, int low, int high) {
        if (low == high) {
            return low;
        } else {
            int mid1 = (low + high) / 2;
            int mid2 = mid1 + 1;
            if (nums[mid1] > nums[mid2]) {
                return helper(nums, low, mid1);
            } else {
                return helper(nums, mid2, high);
            }
        }
    }
}
```
迭代
```Java
class Solution {
    public int findPeakElement(int[] nums) {
        int low = 0;
        int high = nums.length-1;
        
        while (low < high) {
            int mid1 = (low+high)/2;
            int mid2 = mid1+1;
            if(nums[mid1] < nums[mid2])
                low = mid2;
            else
                high = mid1;
        }
        return low;
    }
}
```