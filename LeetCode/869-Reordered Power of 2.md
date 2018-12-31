# 869. Reordered Power of 2

> Starting with a positive integer N, we reorder the digits in any order (including the original order) such that the leading digit is not zero.
>
> Return true if and only if we can do this in a way such that the resulting number is a power of 2.

给一个正整数N，把该整数所有位数以任意顺序改变（不能有前置0），问该数是否是2的幂。

解题思路：

这题主要是利用String的排序。

最初考虑的是，排序后如果有前置0怎么办；

但我们这里相当于对所有2的幂做一个枚举（用位操作来算），并对所有2的幂进行排序，然后与我们的输入进行对比。这样就不用考虑前置0的问题。

```Java
class Solution {
    public boolean reorderedPowerOf2(int N) {
        char[] a1 = String.valueOf(N).toCharArray();
        Arrays.sort(a1);
        String s1 = new String(a1);
        
        for (int i = 0; i < 31; i++) {
            char[] a2 = String.valueOf((int)(1 << i)).toCharArray();
            Arrays.sort(a2);
            String s2 = new String(a2);
            if (s1.equals(s2)) return true;
        }
        
        return false;
    }
}
```
