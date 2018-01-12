### 13. Roman to Integer

> Given a roman numeral, convert it to an integer.

> Input is guaranteed to be within the range from 1 to 3999.

解题思路:
* 把罗马表示法放在一个字典里
* 遍历字符串
* 若左边的字符比右边的小,说明要减去
* 大,则加上
* 返回所得的值

```Python
class Solution(object):
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        roman = {'M': 1000,'D': 500 ,'C': 100,'L': 50,'X': 10,'V': 5,'I': 1}
        z = 0
        for i in range(0, len(s) - 1):
            if roman[s[i]] < roman[s[i+1]]:
                z -= roman[s[i]]
            else:
                z += roman[s[i]]
        return z + roman[s[-1]]
```