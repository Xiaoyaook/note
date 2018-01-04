### 8. String to Integer (atoi)

> Implement atoi to convert a string to an integer.

重点考虑各种边界情况:
* 输入为空字符串
* 移除空格
* 处理符号(正或者负)
* 避免溢出

> Overflow explanation: Integer.MAX_VALUE = 2147483647 and Integer.MIN_VALUE = -2147483648

```Java
class Solution {
    public int myAtoi(String str) {
        int i = 0;
        str = str.trim();        
        char[] c = str.toCharArray();

        int sign = 1;
        if (i < c.length && (c[i] == '-' || c[i] == '+')) {
            if (c[i] == '-') {
                sign = -1;
            }
            i++;
        }      

        int num = 0;
        int bound = Integer.MAX_VALUE / 10;
        while (i < c.length && c[i] >= '0' && c[i] <= '9') {
            int digit = c[i] - '0';
            if (num > bound || (num == bound && digit > 7)) {
                return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            }
            num = num * 10 + digit;
            i++;
        }
        return sign * num;
    }
}
```

处理空格还可以用`while(str.charAt(index) == ' ' && index < str.length())
        index ++;`