# LRU Cache实现

在LeetCode146题作为Hard难度的题目出现

> Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.
>
> get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
>
> put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

## 最近最少使用缓存的回收

为了实现缓存回收，我们需要很容易做到：

* 查询出最近最晚使用的项
* 给最近使用的项做一个标记

链表可以实现这两个操作。检测最近最少使用的项只需要返回链表的尾部。标记一项为最近使用的项只需要从当前位置移除，然后将该项放置到头部。比较困难的事情是怎么快速的在链表中找到该项。

## 哈希表的帮助

哈希表可以在（消耗）常量的时间内索引到某个对象。如果我们创建一个形如**key->链表节点**的哈希表，我们就能够在常量时间内找到最近使用的节点。更甚的是，我们也能够在常量时间内判断节点的是否存在（或不存在）。

找到这个节点后，我们就能将这个节点移动到链表的最前端，标记为最近使用的项了。

## 使用LinkedHashMap实现

我们需要在哈希表的基础上建立一个链表。但是Java已经为我们提供了这种形式的数据结构：**LinkedHashMap**。它甚至提供可覆盖回收策略的方法([removeEldestEntry](https://docs.oracle.com/javase/7/docs/api/java/util/LinkedHashMap.html#removeEldestEntry(java.util.Map.Entry)))。唯一需要我们注意的事情是，改链表的顺序是插入的顺序，而不是访问的顺序。但是，有一个构造函数提供了一个选项，可以使用访问的顺序([LinkedHashMap](https://docs.oracle.com/javase/7/docs/api/java/util/LinkedHashMap.html#LinkedHashMap(int,%20float,%20boolean)))。

这里的实现直接继承**LinkedHashMap<K, V>**，并重写**removeEldestEntry**方法，这个重写的方法是文档中的示例方法

```Java
import java.util.LinkedHashMap;
import java.util.Map;
 
public LRUCache<K, V> extends LinkedHashMap<K, V> {
  private int cacheSize;
 
  public LRUCache(int cacheSize) {
    super(16, 0.75f, true);
    this.cacheSize = cacheSize;
  }

  @Override
  protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
    return size() > cacheSize;
  }
}
```

## 更多实现

这篇博文中提供了三种方法[动手实现一个 LRU cache](https://crossoverjie.top/2018/04/07/algorithm/LRU-cache/)，其中第三种也是利用LinkedHashMap实现。

---

参考自：
[10行Java代码实现最近被使用（LRU）缓存](http://www.importnew.com/16264.html)