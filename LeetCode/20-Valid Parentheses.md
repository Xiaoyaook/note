### 20. Valid Parentheses

> Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

> The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

考察对栈的应用,直接使用Java的Stack类.

算法的时间复杂度是O(n)，空间复杂度也是O(n)

解题思路:
* 遍历传入的String s，如果遇到左括号就入栈；如果遇到右括号，检查栈如果为空，证明不能匹配，如果栈不空，pop出栈顶的元素，看是否与当前的右括号匹配.
* 如果匹配，继续向下进行新一轮的循环，如果不匹配，返回false.
* 全部字符都检查完了以后，判断栈是否为空，空则正确都匹配，不空则证明有没匹配的.
* 检查字符是用==，检查String是用`equals`方法，因为String是引用类型，值相等但是地址可能不等.

```Java
class Solution {
    public boolean isValid(String s) {
        if(s.length() == 0 || s.length() == 1){
            return false;
        }
        
        Stack<Character> stack = new Stack<Character>();
        
        for(int i=0;i<s.length();i++){
            char cur = s.charAt(i);
            if(cur == '(' || cur == '{' || cur == '['){
                stack.push(cur);
            }else{
                switch(cur){
                    case ')': 
                        if(stack.isEmpty() || stack.pop() != '('){
                            return false;
                        } 
                        break;
                    case ']' : 
                        if(stack.isEmpty() || stack.pop() != '['){
                            return false;
                        } 
                        break;
                    case '}' : 
                        if(stack.isEmpty() || stack.pop() != '{'){
                            return false;
                        } 
                        break;
                    default :
                        break;
                }
            }
        }
        
        return stack.size() == 0;
    }
}
```