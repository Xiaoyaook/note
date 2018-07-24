# 快速排序及patition应用

## 快速排序

这是 *Algorithms, 4th Edition* 中的实现方法

```Java
// quicksort the subarray from a[lo] to a[hi]
    private static void sort(Comparable[] a, int lo, int hi) { 
        if (hi <= lo) return;
        int j = partition(a, lo, hi);
        sort(a, lo, j-1);
        sort(a, j+1, hi);
        assert isSorted(a, lo, hi);
    }

    // partition the subarray a[lo..hi] so that a[lo..j-1] <= a[j] <= a[j+1..hi]
    // and return the index j.
    private static int partition(Comparable[] a, int lo, int hi) {
        int i = lo;
        int j = hi + 1;
        Comparable v = a[lo];
        while (true) { 

            // find item on lo to swap
            while (less(a[++i], v)) {
                if (i == hi) break;
            }

            // find item on hi to swap
            while (less(v, a[--j])) {
                if (j == lo) break;      // redundant since a[lo] acts as sentinel
            }

            // check if pointers cross
            if (i >= j) break;

            exch(a, i, j);
        }

        // put partitioning item v at a[j]
        exch(a, lo, j);

        // now, a[lo .. j-1] <= a[j] <= a[j+1 .. hi]
        return j;
    }

    /***************************************************************************
    *  Helper sorting functions.
    ***************************************************************************/
    
    // is v < w ?
    private static boolean less(Comparable v, Comparable w) {
        if (v == w) return false;   // optimization when reference equals
        return v.compareTo(w) < 0;
    }
        
    // exchange a[i] and a[j]
    private static void exch(Object[] a, int i, int j) {
        Object swap = a[i];
        a[i] = a[j];
        a[j] = swap;
    }
    ```

## Partition 的应用

由上面的实现可知，Partition 可以实现 a[lo..j-1] <= a[j] <= a[j+1..hi]

### Top K 问题

例如找到数组中第 k 小的元素，我们就可以利用 Partition 函数，不过这一次我们不再需要对我们选取的 pivot 的左边和右边同时递归

* 当 pivot 在数组中的位置等于 k-1 时，此时我们就得到了结果
* 当 pivot 在数组中的位置大于 k-1 时，说明我们的范围超了，只递归 pivot 左边就行
* 当 pivot 在数组中的位置小于 k-1 时，我们递归 pivot 的右边
* 最坏的时间复杂度O(n**2)，不过平均来说是O(n)

```Java
 // This function returns k'th smallest element 
    // in arr[l..r] using QuickSort based method. 
    // ASSUMPTION: ALL ELEMENTS IN ARR[] ARE DISTINCT
    public static int kthSmallest(Integer[] arr, int l, 
                                         int r, int k)
    {
        // If k is smaller than number of elements
        // in array
        if (k > 0 && k <= r - l + 1)
        {
            // Partition the array around last 
            // element and get position of pivot 
            // element in sorted array
            int pos = partition(arr, l, r);
 
            // If position is same as k
            if (pos-l == k-1)
                return arr[pos];
             
            // If position is more, recur for
            // left subarray
            if (pos-l > k-1) 
                return kthSmallest(arr, l, pos-1, k);
 
            // Else recur for right subarray
            return kthSmallest(arr, pos+1, r, k-pos+l-1);
        }
 
        // If k is more than number of elements
        // in array
        return Integer.MAX_VALUE;
    }
```