# Pow(x, n)

Implement pow(x, n), which calculates x raised to the power n (x ^ n).

> -100.0 < x < 100.0
n is a 32-bit signed integer, within the range [−231, 231 − 1]

求指数的问题，注意指数为负的情况。注意负数溢出的情况。


## 解题思路

下面提供几种解法：

递归
```
class Solution {
    public double myPow(double x, int n) {
        if(n == 0)
            return 1;
        if(n<0){
            return 1/x * myPow(1/x, -(n + 1));
        }
        return (n%2 == 0) ? myPow(x*x, n/2) : x*myPow(x*x, n/2);
    }
}
```

一种整洁的写法
```Java
double myPow(double x, int n) {
    if(n<0) return 1/x * myPow(1/x, -(n+1));
    if(n==0) return 1;
    if(n==2) return x*x;
    if(n%2==0) return myPow( myPow(x, n/2), 2);
    else return x*myPow( myPow(x, n/2), 2);
}
```