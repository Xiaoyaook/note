### 7. Reverse Integer
> Given a 32-bit signed integer, reverse digits of an integer.

> Assume we are dealing with an environment which could only hold integers within the 32-bit signed integer range. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

解题的重点是用好:取余,整除.并对返回值判断是否大于32位整数范围
>　int: 32位整数 -2,147,483,648——2,147,483,647

> long: 64位整数 -9,223,372,036,854,775,808——9,223,372,036,854,775,807

```Java
class Solution {
    public int reverse(int x) {
        long rev = 0;
        while (x != 0) {
            rev = rev * 10 + x % 10;
            x = x / 10;
            if (rev > Integer.MAX_VALUE || rev < Integer.MIN_VALUE) {
                return 0;
            }
        }
        return (int) rev;
    }
}
```
