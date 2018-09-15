# ConcurrentHashMap源码分析

Java 7中的ConcurrentHashMap的底层数据结构仍然是数组和链表。与HashMap不同的是，ConcurrentHashMap最外层不是一个大的数组，而是一个Segment的数组。每个Segment包含一个与HashMap数据结构差不多的链表数组。

在读写某个Key时，先取该Key的哈希值。并将哈希值的高N位对Segment个数取模从而得到该Key应该属于哪个Segment，接着如同操作HashMap一样操作这个Segment。

Segment继承自ReentrantLock，所以我们可以很方便的对每一个Segment上锁。

[ConcurrentHashMap底层原理](https://juejin.im/post/5aba1030f265da23961269c6#heading-8)

[JDK1.8源码分析之ConcurrentHashMap（一）](https://www.cnblogs.com/leesf456/p/5453341.html)

[探索jdk8之ConcurrentHashMap 的实现机制](http://www.cnblogs.com/huaizuo/archive/2016/04/20/5413069.html)