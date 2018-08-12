# Generate Parentheses

> Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

给一个整数n，列举出n对括号的所以可能性。

解题思路：

> 给出一个整数n，那么生成的合法括号串的长度是2 * n，那么运用递归去枚举每个位置上的可能出现的字符(，)，枚举的时候要注意，当前位置可以放(，但是能不能放)就要看到目前为止放了多少个)，如果(的个数比)多，那么当前位置就可以放)，这其实和用栈验证括号串的合法性的思想是一样的。

```Java
 public List<String> generateParenthesis(int n) {
        List<String> list = new ArrayList<String>();
        backtrack(list, "", 0, 0, n);
        return list;
    }
    
    public void backtrack(List<String> list, String str, int open, int close, int max){
        
        if(str.length() == max*2){
            list.add(str);
            return;
        }
        
        if(open < max)
            backtrack(list, str+"(", open+1, close, max);
        if(close < open)
            backtrack(list, str+")", open, close+1, max);
    }
```

```Java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList<>();
        dfs(n, n, res, new StringBuilder());
        return res;
    }
    
    void dfs(int open, int close, List<String> res, StringBuilder sb){
        if(close<open || open<0 || close<0) return;
        
        if(open==0 && close==0){
            res.add(sb.toString());
            return;
        }        
        
        sb.append("(");
        dfs(open-1, close, res, sb);
        sb.setLength(sb.length()-1);

        sb.append(")");
        dfs(open, close-1, res, sb);
        sb.setLength(sb.length()-1);

    }
}
```

```Python
class Solution(object):
    def backtrack(self, n, score, cur, ans):
        if len(cur) > n*2 or score < 0:
            return
        if len(cur) == n*2 and score == 0:
            ans.append(cur)
            return
        
        self.backtrack(n, score + 1, cur + "(", ans)
        self.backtrack(n, score - 1, cur + ")", ans)
        
    def generateParenthesis(self, n):
        ans = []
        self.backtrack(n, 0, "", ans)
        return ans
```