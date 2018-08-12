# Group Anagrams

Given an array of strings, group anagrams together.

给一个字符串数组，找出字符相同但位置不同的字符串归类。

> Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]

## 解题思路

有一个利用hash的解法，很巧妙

取26个素数，每个字母分别对应一个素数。那么一个字符串，无论字符顺序如何，相乘的结果都是相同的，我们取结果为key。

hashmap存放的是key与`List<String>`的映射。每一个字符串，先看是否能在hashmap中取到，否则新建一个`List<String>`并对应起来。

这个解法最需要注意的是存在哈希碰撞。

```Java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
           int[] prime = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101,103};//最多10609个z
    
            List<List<String>> res = new ArrayList<>();
            HashMap<Integer, Integer> map = new HashMap<>();
            for (String s : strs) {
                int key = 1;
                for (char c : s.toCharArray()) {
                    key *= prime[c - 'a'];
                }
                List<String> t;
                if (map.containsKey(key)) {
                    t = res.get(map.get(key));
                } else {
                    t = new ArrayList<>();
                    res.add(t);
                    map.put(key, res.size() - 1);
                }
                t.add(s);
            }
            return res;
    }
}
```