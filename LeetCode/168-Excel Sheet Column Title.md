# Excel Sheet Column Title

Given a positive integer, return its corresponding column title as appear in an Excel sheet.

具体看题中的例子吧，大概就是用A-Z表示26进制。

## 解题思路
题目中1 -> A，而我们这里
0 -> A, 25 -> Z 故下面的n需要减1

要注意的地方就是我们是从低位开始算的，也就是说我们算的第一个数相当于个位，之后以此类推。

```Java
class Solution {
    public String convertToTitle(int n) {
        String res = "";
        while(n != 0) {
            char ch = (char)('A' + ((n - 1) % 26));
            n = (n - 1) / 26;
            res = ch + res;
        }
        return res;
    }
}
```