# Sliding Window Maximum

> Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

> You may assume k is always valid, 1 ≤ k ≤ input array's size for non-empty array.

滑动窗口，返回每一次窗口的最大值。

剑指offer 65题

## 解题思路

先给个结论：滑动窗口的最大值永远在队列的头部。

结果的长度为：数组的长度-k+1。

用LinkedList做双向队列。

下面的代码中有注释。注意我们队列中存储的都是index值，比较时，需要用index从数组中取数。

```Java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums==null||nums.length==0)
            return new int[0];

        int[] result = new int[nums.length-k+1];

        LinkedList<Integer> deque = new LinkedList<Integer>();
        // 注意我们队列里储存的都是index值
        for(int i=0; i<nums.length; i++){
            // 当队列不为空，且队列第一个元素，等于i-k时，弹出第一个元素
            if(!deque.isEmpty()&&deque.peekFirst()==i-k) 
                deque.poll();

            // 当队列不为空，且数组最后一个元素，小于当前元素时，把队列最后一个元素删去
            while(!deque.isEmpty()&&nums[deque.peekLast()]<nums[i]){
                deque.removeLast();
            }    

            deque.offer(i);

            if(i+1>=k)
                result[i+1-k]=nums[deque.peek()];
        }

        return result;
    }
}
```

还有一种巧妙的解法：

> For Example: A = [2,1,3,4,6,3,8,9,10,12,56], w=4
>
>partition the array in blocks of size w=4. The last block may have less then w.
2, 1, 3, 4 | 6, 3, 8, 9 | 10, 12, 56|
>
>Traverse the list from start to end and calculate max_so_far. Reset max after each block boundary (of w elements).
left_max[] = 2, 2, 3, 4 | 6, 6, 8, 9 | 10, 12, 56
>
>Similarly calculate max in future by traversing from end to start.
right_max[] = 4, 4, 4, 4 | 9, 9, 9, 9 | 56, 56, 56
>
>now, sliding max at each position i in current window, sliding-max(i) = max{right_max(i), left_max(i+w-1)}
sliding_max = 4, 6, 6, 8, 9, 10, 12, 56

先把原数组以k个为一组，分割成若干个。

从左往右循环，若前一个元素比当前元素大，则进行替换。到边界时，则最大值重置。

从右往左循环，若前一个元素比当前元素大，则进行替换。到边界时，则最大值重置。

这样得到两个新数组。

开始计算结果，因为结果数为 `数组的长度-k+1` 个，
故循环`数组的长度-k+1`次

当前滑动窗口的最大值为`sliding-max(i) = max{left_max(i+w-1)， right_max(i)}`

```Java
 public static int[] slidingWindowMax(final int[] in, final int w) {
    final int[] max_left = new int[in.length];
    final int[] max_right = new int[in.length];

    max_left[0] = in[0];
    max_right[in.length - 1] = in[in.length - 1];

    for (int i = 1; i < in.length; i++) {
        max_left[i] = (i % w == 0) ? in[i] : Math.max(max_left[i - 1], in[i]);

        final int j = in.length - i - 1;
        max_right[j] = (j % w == 0) ? in[j] : Math.max(max_right[j + 1], in[j]);
    }

    final int[] sliding_max = new int[in.length - w + 1];
    for (int i = 0, j = 0; i + w <= in.length; i++) {
        sliding_max[j++] = Math.max(max_right[i], max_left[i + w - 1]);
    }

    return sliding_max;
}
```