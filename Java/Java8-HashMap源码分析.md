## Java8 HashMap源码分析

数据结构: 数组 + 链表 + 红黑树(JDK1.8增加了红黑树部分),链表长度大于8时转化为红黑树.

hashCode方法定义在Objects类中,因此每个对象都有一个默认的散列码,其值为对象的储存地址.

```Java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable
```
HashMap继承自父类（AbstractMap），实现了Map、Cloneable、Serializable接口

```Java
// 实现Serializable接口的类都有一个表示序列化版本标识符的静态变量
private static final long serialVersionUID = 362498820763181265L;

```

HashMap的默认构造函数源码
```Java
public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }
```
可知HashMap对大部分字段都有一个默认值,以下几个字段比较关键
```Java
     int threshold;             // 所能容纳的key-value对极限 
     final float loadFactor;    // 负载因子
     int modCount;  
     int size;  // 实际存在的键值对数量
```

### 部分功能实现方法

#### hash实现
```Java
static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```
没有直接使用key的hashcode()，而是使key的hashcode()高16位不变，低16位与高16位异或作为最终hash值.

#### put实现
```Java
public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
```
返回了调用的`putVal`方法来实现,这里贴出来太杂乱,详细看源码

#### resize实现
重新计算容量