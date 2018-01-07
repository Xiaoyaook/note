### 14. Longest Common Prefix

> Write a function to find the longest common prefix string amongst an array of strings.

解题思路,是设第一个为基准字符串,不断地检查其是否为之后的元素的子串,且从index 0开始,不然将该字符串的长度减一.

```Java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs.length == 0) return "";
        String pre = strs[0];
        for (int i = 1; i < strs.length; i++)
            while(strs[i].indexOf(pre) != 0)
                pre = pre.substring(0,pre.length()-1);
        return pre;
    }
}
```

Python的方法更容易理解,使用了Python的高阶函数reduce,从第一个字符串开始不断去跟之后的字符串比较,切片
```Python
class Solution(object):
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        if not strs:
            return ''
        else:
            return reduce(self.lcp, strs)
        
    def lcp(self, str1, str2):
        i = 0
        while i < len(str1) and i < len(str2):
            if str1[i] == str2[i]:
                i += 1
            else:
                break
        return str1[:i]
```