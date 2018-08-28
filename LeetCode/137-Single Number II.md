# Single Number II

> Given a non-empty array of integers, every element appears three times except for one, which appears exactly once. Find that single one.

> Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

一个非空数组，里面的每个元素都将出现三次，除了一个只出现一次的元素，在线性时间内找到它，并不使用多余的空间。

## 解题思路

出现三次比出现两次要复杂些，出现两次时我们只用异或就行。

出现三次，我们用两个int来保存当前值。

ones用来保存出现一次的元素，twos保存出现两次的元素。当元素出现三次时，ones，twos**相对于该元素**为0。

最后ones的值即为只出现了一次的元素。

```Java
class Solution {
    public int singleNumber(int[] nums) {
            int ones = 0, twos = 0;
        for(int i = 0; i < nums.length; i++){
            ones = (ones ^ nums[i]) & ~twos;
            twos = (twos ^ nums[i]) & ~ones;
        }
        return ones;
    }
}
```