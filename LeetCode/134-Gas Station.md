# Gas Station

> There are N gas stations along a circular route, where the amount of gas at station i is gas[i].
>
> You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.
>
> Return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.

> If there exists a solution, it is guaranteed to be unique.
> Both input arrays are non-empty and have the same length.
> Each element in the input arrays is a non-negative integer.

一圈加油站，每站有一定数量的汽油，每站到下一站需要消耗一定量的汽油。任选一个作为起点，沿顺时针方向是否能走一圈？

## 解题思路

算是暴力一些的方法，从第一个汽油站开始，每一个汽油站都尝试作为起点，看是否能转一圈。

注意转一圈所需要的次数是一定的，等于加油站的数量。

故以上是两个for循环嵌套。

```Java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int n = gas.length;

        for (int i = 0; i < n; i++) {
            int curGas = 0;
            int curIndex = i;
            int tempIndex = i;
            for (int j = 0; j < n; j++) {
                curGas += gas[tempIndex];
                curGas -= cost[tempIndex];
                
                if (curGas < 0) break;
                
                if (j == n -1) return curIndex;
                if (tempIndex + 1 >= n) {
                    tempIndex = 0;
                } else {
                    tempIndex++;
                }
                
            }
        }
        return -1;
    }
}
```