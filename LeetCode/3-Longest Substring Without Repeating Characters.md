### 3. Longest Substring Without Repeating Characters

> Given a string, find the length of the longest substring without repeating characters.

解题思路
* 最关键的是用字典记录每个字符出现的index,如果已经出现过该字符,则将index刷新.
* 用start记录子串的起始位置.

时间复杂度O(n)

```Java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int start = 0;
        int maxLength = start;
        HashMap<Character,Integer> usedChar = new HashMap<>();
        for (int i=0; i<s.length(); i++) {
            if (usedChar.containsKey(s.charAt(i)) && start <= usedChar.get(s.charAt(i))) {
                start = usedChar.get(s.charAt(i)) + 1;
            } else {
                maxLength = Math.max(maxLength, i-start+1);
            }
            usedChar.put(s.charAt(i),i);
        }
        return maxLength;
    }
}
```

```Python
class Solution:
    def lengthOfLongestSubstring(self, s):
        start = maxLength = 0
        usedChar = {}

        for i in range(len(s)):
            if s[i] in usedChar and start <= usedChar[s[i]]:
                start = usedChar[s[i]] + 1
            else:
                maxLength = max(maxLength, i - start + 1)

            usedChar[s[i]] = i

        return maxLength
```