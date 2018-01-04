### 9. Palindrome Number

> Determine whether an integer is a palindrome. Do this without extra space.

需要考虑:
* 输入值为负数的情况.
* 如果想把整数转为字符串,注意题目中所要求的不能有额外空间.
* 直接反转数字的话,数字出现溢出怎么办?

解题思路:还是选择直接反转数字,当反转的数字溢出时,它将是一个负数,与x比较时会返回false.
```Java
class Solution {
    public boolean isPalindrome(int x) {
        int palindromeX = 0;
        int inputX = x;
        while (x>0) {
            palindromeX = palindromeX*10 + x%10;
            x = x/10;
        }
        return palindromeX == inputX;
    }
}
```