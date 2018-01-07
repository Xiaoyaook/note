## Java8 LinkedList源码分析

```Java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
```
`LinkedList`继承自`AbstractSequentialList`,实现了`List<E>, Deque<E>, Cloneable, java.io.Serializable`接口

实现了Deque接口所以LinkedList是一个双端队列

#### LinkedList构造函数
```Java
public LinkedList() {
    }

public LinkedList(Collection<? extends E> c) {
        // 调用无参构造函数
        this();
        // 添加集合中所有的元素
        addAll(c);
    }
```

#### 类的属性
```Java
    // 实际元素个数
    transient int size = 0;
    // 头结点
    transient Node<E> first;
    // 尾结点
    transient Node<E> last;
```
LinkedList的属性非常简单，一个头结点、一个尾结点、一个表示链表中实际元素个数的变量。注意，用transient关键字修饰意味着在序列化时该域是不会序列化的.

#### 类的内部类
```Java
private static class Node<E> {
        E item;  // // 数据域
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```
注意这是一个静态内部类,类Node就是实际的结点，用于存放实际元素的地方。

#### 类的方法