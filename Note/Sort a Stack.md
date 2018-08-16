# Sort a Stack

对一个栈中的数据排序，只能使用push pop peek empty四种操作，并且不能手动申请额外的内存。

## insert

用栈实现选择排序，关键的步骤是insert函数：向一个已经有序的栈插入元素e，并保持有序性。由于不能手动申请新内存，可以用系统栈的**递归调用**实现。

这里拿整数举例子，实际实现可以使用范型（实现了Comparable接口的）

```Java
void insert(Stack<Integer> s, int e) {
    if (s.empty() || s.peek() <= e) {
        s.push(e);
	} else {
		Integer f = s.peek();
		s.pop();
		insert(s, e);
		s.push(f);
	}
}
```

## sort

排序函数也同样是**递归实现**，将e插入栈底已经排好的序列中。

```Java
void sort(Stack<Integer> s) {
    if (!s.empty()) {
		int e = s.peek();
		s.pop();
		sort(s);
		insert(s, e);
	}
} 
```

以上两个函数均使用了O(N)的系统栈内存，总时间复杂度为O(N^2)。

---
[Sort a Stack](http://devhui.com/2016/06/06/sort-a-stack/)