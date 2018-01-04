### 6. ZigZag Conversion

解题思路:
* 首先把要保存结果的数组创建出来
* 然后根据所要求的行数,从上往下,再从下往上不断往数组中添加
* 最后把数组连起来,转化成字符串

注意从下往斜上方走的时候的起始位置和条件`idx = numRows-2`
```Java
class Solution {
    public String convert(String s, int numRows) {
        char[] c = s.toCharArray();
        int len = c.length;
        StringBuffer[] sb = new StringBuffer[numRows];
        for (int i = 0; i<numRows; i++) sb[i] = new StringBuffer();
        
        int i = 0;
        while (i < len) {
            for (int idx = 0; idx < numRows && i < len; idx++) // 从上往下
                sb[idx].append(c[i++]);
            for (int idx = numRows-2; idx >= 1 && i < len; idx--) // 从下往斜上
                sb[idx].append(c[i++]);
        }
        for (int idx = 1; idx < sb.length; idx++)
            sb[0].append(sb[idx]);
        return sb[0].toString();
        }
}
```