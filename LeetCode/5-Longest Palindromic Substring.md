### 5. Longest Palindromic Substring

> Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

使用中心扩展法,时间复杂度为O(n^2)，空间复杂度O(1)

`extendPalindrome()`方法中的参数j和k可以看做两个指针,分别向左移动和向右移动,并判断是否相等.只需要对0到len-1的下标都做此操作，就可以求出最长的回文子串.

注意回文字符串有奇偶对称之分，即"abcba"与"abba"2种类型，所以我们调用两次`extendPalindrome()`分别对应奇偶两种情况.

```Java
class Solution {
    private int lo, maxLen;
    public String longestPalindrome(String s) {
        
        int len = s.length();
        if (len < 2) {
            return s;
        }
        for (int i=0; i < len-1; i++) {
            extendPalindrome(s, i, i); // 长度为奇数
            extendPalindrome(s, i, i+1); // 长度为偶数
        }
        return s.substring(lo, lo + maxLen);
    }
    private void extendPalindrome(String s, int j, int k) {
        while (j >= 0 && k < s.length() && s.charAt(j) == s.charAt(k)) {
            j--;
            k++;
        }
        if (maxLen < k - j - 1) {
            lo = j + 1;
            maxLen = k - j - 1;
        }
    }
}
```