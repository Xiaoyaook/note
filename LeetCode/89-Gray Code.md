# Gray Code

> The gray code is a binary numeral system where two successive values differ in only one bit.
>
>Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

格雷码是指连续的两数字之间只有一位不同（二进制位）。

给一数字n，找出所有位数为n的数字，并按格雷码的形式排列。

## 解题思路

注意序列的长度为 2 ** n，且序列第一位必为0.

本题的关键是，序列的第i位为， G(i) = i ^ (i/2)   

其中 ^ 表示异或，找到这个规律后，题就很简单了。

```Java
class Solution {
    public List<Integer> grayCode(int n) {
        List<Integer> result = new LinkedList<>();
        for (int i = 0; i < 1<<n; i++) result.add(i ^ i>>1);
        return result;
    }
}
```

下面这个反向叠加的Python解法也很有意思，本质是一样的。
还附送一个面试小技巧。。。
> All you need is a bit of careful thought.
>
> Btw, it's extremely useful to write down your thought/demo in comments before you actually start to write the code, especially during interview.
>
> Even if you do not solve the problem finally, the interviewer at least get to know what you're thinking.
>
> And if you don't get the problem right, he/she will have a chance to correct you.

```Python
class Solution:
    # @return a list of integers
    '''
    from up to down, then left to right
    
    0   1   11  110
            10  111
                101
                100
                
    start:      [0]
    i = 0:      [0, 1]
    i = 1:      [0, 1, 3, 2]
    i = 2:      [0, 1, 3, 2, 6, 7, 5, 4]
    '''
    def grayCode(self, n):
        results = [0]
        for i in range(n):
            results += [x + pow(2, i) for x in reversed(results)]
        return results
```