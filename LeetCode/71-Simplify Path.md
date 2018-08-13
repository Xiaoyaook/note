# Simplify Path

> Given an absolute path for a file (Unix-style), simplify it.

> For example,
path = "/home/", => "/home"
path = "/a/./b/../../c/", => "/c"

简化路径

## 解题思路

运用栈，一个挺通用的做法，不光适用本题。

本题，观察简化过程，发现 目录和 . 还有 .. 是不会同时出现的，并且以 / 分割。

同时题目中要求了，//看作一个/，那么我们就用 / 来分割字符串，这样分割后不含 / ，我们后期拼接字符串时再加上。

创建一个栈，储存路径信息。创建一个set，储存我们需要跳过的信息。

循环我们分割的字符串数组，若遇到 .. 且栈不为空，则弹出一个元素。若遇到set中不包含的元素，即我们需要的路径信息，压入栈。

最后从栈中循环取值，拼接字符串。

```Java
class Solution {
    public String simplifyPath(String path) {
        
        Deque<String> stack = new LinkedList<>();
        
        Set<String> skip = new HashSet<>(Arrays.asList("..",".",""));
        
        for (String dir : path.split("/")) {
            if (dir.equals("..") && !stack.isEmpty()) stack.pop();
            else if (!skip.contains(dir)) stack.push(dir);
        }
        String res = "";
        for (String dir : stack) res = "/" + dir + res;
        return res.isEmpty() ? "/" : res;
    }
}
```