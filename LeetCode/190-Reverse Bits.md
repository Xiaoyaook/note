# 190. Reverse Bits

Reverse bits of a given 32 bits unsigned integer.

给一个32位的无符号数，输出其二进制位反转后的数。

## 解题思路

位操作

```Java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int result = 0;
        for (int i = 0; i < 32; ++i) {
            result = result<<1  | (n & 1);
            n >>>= 1;
        }
        return result;
    }
}
```