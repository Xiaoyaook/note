# Fraction to Recurring Decimal

> Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.
>
> If the fractional part is repeating, enclose the repeating part in parentheses.

给出分数的字符串形式，如1/2为0.5。若有重复的小数用括号将该小数括起来。

## 解题思路

要考虑边界情况，如负数，或者int大小溢出的情况。

接下来解题的关键就是，我们把整数部分和小数部分分开来算。

用一个hashmap来保存每一位小数，hash表的key为num，`num %= den;`，value为`res.length()`

```Java
public class Solution {
    public String fractionToDecimal(int numerator, int denominator) {
        if (numerator == 0) {
            return "0";
        }
        StringBuilder res = new StringBuilder();
        // "+" or "-" 判断结果为正还是为负
        res.append(((numerator > 0) ^ (denominator > 0)) ? "-" : "");
        // long型，防止溢出
        long num = Math.abs((long)numerator);
        long den = Math.abs((long)denominator);
        
        // 首先计算整数部分
        res.append(num / den);
        num %= den;
        if (num == 0) {
            return res.toString();
        }
        
        // 小数部分
        res.append(".");
        HashMap<Long, Integer> map = new HashMap<Long, Integer>();
        map.put(num, res.length());
        while (num != 0) {
            // 循环计算小数部分
            num *= 10;
            res.append(num / den);
            num %= den;
            // 如果小数出现重复，则加括号，并结束循环。
            if (map.containsKey(num)) {
                int index = map.get(num);
                res.insert(index, "(");
                res.append(")");
                break;
            }
            else {
                map.put(num, res.length());
            }
        }
        return res.toString();
    }
}
```