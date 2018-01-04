### 11. Container With Most Water

> Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

> Note: You may not slant the container and n is at least 2.

解题思路: 先记录数组长度,从两头往中间走,每次从短板那边向里走一位.

```Java
class Solution {
    public int maxArea(int[] height) {
        int L = 0, R = height.length - 1, width = height.length - 1, res = 0;
        for(int i = width; i > 0; i--) {
            if (height[L] < height[R]) {
                res= Math.max(res, height[L] * i);
                L = L + 1;
            } else {
                res= Math.max(res, height[R] * i);
                R = R - 1;
            }
        }
        return res;
    }
}
```

```Python
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        L, R, width, res = 0, len(height) - 1, len(height) - 1, 0
        for w in range(width, 0, -1):
            if height[L] < height[R]:
                res, L = max(res, height[L] * w), L + 1
            else:
                res, R = max(res, height[R] * w), R - 1
        return res
```