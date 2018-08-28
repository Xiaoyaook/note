# Word Break

> Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.
>
> The same word in the dictionary may be reused multiple times in the segmentation.
You may assume the dictionary does not contain duplicate words.

给一个字符串和一个字典，判断字符串是否能够由字典中的词组成，字典中的词可以重复使用。

## 解题思路

### 超时的暴力解法
这个就是很暴力的方法，遍历每一种情况。

当遇到测试用例为aaaaaaaaaa...，字典为1个a，2个a，等等持续增加时，会超时。

```Java
class Solution {
    public boolean existed = false;
    
    public boolean wordBreak(String s, List<String> wordDict) {
        dfs(s, wordDict);
        return this.existed;
    }

    public void dfs(String s, List<String> wordDict) {
        if (s.length() == 0) this.existed = true;
        
        for (int i = 0; i < wordDict.size(); i++) {
            String word = wordDict.get(i);
            
            if (s.length() < word.length()) {
                continue;
            } else if (s.length() > word.length()) {
                if (varify(s, word)) dfs(s.substring(word.length()), wordDict);
            } else {
                if (varify(s, word)) this.existed = true;
            }
        }
    }
    
    public boolean varify(String s1, String s2) {
        for (int i = 0; i < s2.length(); i++) {
            if (s1.charAt(i) != s2.charAt(i)) return false;
        }
        return true;
    }
}
```

### 精简的解法

避免了重复的情况

```Java
class Solution {
    Set<String> map = new HashSet();
    public boolean wordBreak(String s, List<String> wordDict) {
        if(wordDict.contains(s)) return true;
        if(map.contains(s)) return false;
        for(String word : wordDict){
            if(s.startsWith(word)){
                if(wordBreak(s.substring(word.length()), wordDict)) return true;
            }
        }
        map.add(s);
        return false;
    }  
}
```