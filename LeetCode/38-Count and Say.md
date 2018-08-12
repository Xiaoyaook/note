# Count and Say

The count-and-say sequence is the sequence of integers with the first five terms as following:

1. 1
2. 11
3. 21
4. 1211
5. 111221

1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.
Given an integer n, generate the nth term of the count-and-say sequence.

Note: Each term of the sequence of integers will be represented as a string.

## 解题思路

首先分析规律，然后利用递归循环即可，注意边界情况。

```Java
class Solution {
    public String countAndSay(int n) {
        if (n == 1) {
            return "1";
        }
        if (n == 2) {
            return "11";
        }
        return say("11", n-2);
    }
    
    public String say(String str, int num) {
        if (num == 0) return str;
        
        String res = "";
        int count = 1;
        char cur = str.charAt(0);
        for (int i = 1; i < str.length(); i++) {
            if (str.charAt(i) != cur) {
                res = res + count + cur;
                count = 1;
                cur = str.charAt(i);
            } else {
                count += 1;
            }
            if (i == str.length() - 1) {
                res = res + count + cur;
                return say(res, num-1);
            }
        }
        return res;
    }
}
```
优雅一些的版本
```Java
class Solution {
    public String countAndSay(int n){
        if(n == 1) return "1";
        return count(countAndSay(n-1));
    }

    private String count(String s){
        StringBuilder sb = new StringBuilder();
        int k = 0;
        for (int j = 0; j < s.length(); j++) {
            if(s.charAt(j) != s.charAt(k) ){
                sb.append(j-k);
                sb.append(s.charAt(k));
                k = j;
            }
            if(j == s.length() - 1){
                sb.append(j-k + 1);
                sb.append(s.charAt(k));
            }
        }
        return sb.toString();
    }
}
```