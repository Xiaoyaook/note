### Java8 HashMap源码分析

数据结构: 数组 + 链表 + 红黑树(JDK1.8增加了红黑树部分)

hashCode方法定义在Objects类中,因此每个对象都有一个默认的散列码,其值为对象的储存地址.

```Java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable
```
HashMap继承自父类（AbstractMap），实现了Map、Cloneable、Serializable接口

实现Serializable接口的类都有一个表示序列化版本标识符的静态变量