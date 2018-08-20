# Java基础问题

## 如何使用反射机制获取私有字段，并且改变字段的值

可以通过类对象的getDeclaredField()方法，获取字段（Field）对象，然后再通过字段对象的setAccessible(true)将其设置为可以访问，接下来就可以通过get/set方法来获取/设置字段的值了。下面的代码实现了一个反射的工具类，其中的两个静态方法分别用于获取和设置私有字段的值，字段可以是基本类型也可以是对象类型且支持多级对象操作，例如ReflectionUtil.get(dog, "owner.car.engine.id");可以获得dog对象的主人的汽车的引擎的ID号。
 
getDeclaredField(String name) ：
          返回一个 Field 对象，该对象反映此 Class 对象所表示的类或接口的指定已声明字段。

```Java 
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Modifier;
import java.util.ArrayList;
import java.util.List;
 
/**
 * 反射工具类
 * @author 骆昊
 *
 */
public class ReflectionUtil {
 
    private ReflectionUtil() {
        throw new AssertionError();
    }
 
    /**
     * 通过反射取对象指定字段(属性)的值
     * @param target 目标对象
     * @param fieldName 字段的名字
     * @throws 如果取不到对象指定字段的值则抛出异常
     * @return 字段的值
     */
    public static Object getValue(Object target, String fieldName) {
        Class<?> clazz = target.getClass();
        String[] fs = fieldName.split("\\.");
 
        try {
            for(int i = 0; i < fs.length - 1; i++) {
                Field f = clazz.getDeclaredField(fs[i]);
                f.setAccessible(true);
                target = f.get(target);
                clazz = target.getClass();
            }
 
            Field f = clazz.getDeclaredField(fs[fs.length - 1]);
            f.setAccessible(true);
            return f.get(target);
        }
        catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
 
    /**
     * 通过反射给对象的指定字段赋值
     * @param target 目标对象
     * @param fieldName 字段的名称
     * @param value 值
     */
    public static void setValue(Object target, String fieldName, Object value) {
        Class<?> clazz = target.getClass();
        String[] fs = fieldName.split("\\.");
        try {
            for(int i = 0; i < fs.length - 1; i++) {
                Field f = clazz.getDeclaredField(fs[i]);
                f.setAccessible(true);
                Object val = f.get(target);
                if(val == null) {
                    Constructor<?> c = f.getType().getDeclaredConstructor();
                    c.setAccessible(true);
                    val = c.newInstance();
                    f.set(target, val);
                }
                target = val;
                clazz = target.getClass();
            }
 
            Field f = clazz.getDeclaredField(fs[fs.length - 1]);
            f.setAccessible(true);
            f.set(target, value);
        }
        catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
 
}
```
```Java
--------------------------通过反射调用对象的方法？--------------------------

import java.lang.reflect.Method;
 
class MethodInvokeTest {
 
    public static void main(String[] args) throws Exception {
        String str = "hello";
        Method m = str.getClass().getMethod("toUpperCase");
        System.out.println(m.invoke(str));  // HELLO
    }

```
[通过反射获取和设置对象私有字段的值？](https://blog.csdn.net/u012726702/article/details/72027028)

## HashMap的长度为什么要是2的n次方

HashMap为了存取高效，要尽量较少碰撞，就是要尽量把数据分配均匀，每个链表长度大致相同，这个实现就在把数据存到哪个链表中的算法；
这个算法实际就是取模，hash%length，计算机中直接求余效率不如位移运算，源码中做了优化hash&(length-1)，
hash%length==hash&(length-1)的前提是length是2的n次方；

## java中为什么要实现序列化，什么时候实现序列化？

序列化就是一种用来处理对象流的机制，所谓对象流也就是将对象的内容进行流化,将数据分解成字节流，以便存储在文件中或在网络上传输。可以对流化后的对象进行读写操作，也可将流化后的对象传输于网络之间。序列化是为了解决在对对象流进行读写操作时所引发的问题。 序列化的实现：将需要被序列化的类实现Serializable接口，该接口没有需要实现的方法，implements Serializable只是为了标注该对象是可被序列化的，然后使用一个输出流(如：FileOutputStream)来构造一个ObjectOutputStream(对象流)对象，接着，使用ObjectOutputStream对象的writeObject(Object obj)方法就可以将参数为obj的对象写出(即保存其状态)，要恢复的话则用输入流;

序列化分为两大部分：序列化和反序列化。序列化是这个过程的第一部分，将数据分解成字节流，以便存储在文件中或在网络上传输。反序列化就是打开字节流并重构对象。对象序列化不仅要将基本数据类型转换成字节表示，有时还要恢复数据。恢复数据要求有恢复数据的对象实例 

## JDK 源码中 HashMap 的 hash 方法原理是什么？

```Java
static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```

其中，key.hashCode（）是Key自带的hashCode()方法，返回一个int类型的散列值。我们大家知道，32位带符号的int表值范围从-2147483648到2147483648。这样只要hash函数松散的话，一般是很难发生碰撞的，因为HashMap的初始容量只有16。但是这样的散列值我们是不能直接拿来用的。用之前需要对数组的长度取模运算。得到余数才是索引值。我们来看下HashMap中怎么实现的。

```Java
int index = hash & (arrays.length-1);
```

那么这也就明白了为什么HashMap的数组长度是2的整数幂。比如以初始长度为16为例，16-1 = 15，15的二进制数位00000000 00000000 00001111。可以看出一个基数二进制最后一位必然位1，当与一个hash值进行与运算时，最后一位可能是0也可能是1。但偶数与一个hash值进行与运算最后一位必然为0，造成有些位置永远映射不上值。

但是这时，又出现了一个问题，即使散列函数很松散，但只取最后几位碰撞也会很严重。这时候hash算法的价值就体现出来了。

hashCode右移16位，正好是32bit的一半。与自己本身做异或操作（相同为0，不同为1）。就是为了混合哈希值的高位和地位，增加低位的随机性。并且混合后的值也变相保持了高位的特征。

---
[浅谈HashMap中的hash算法](http://ibat.xyz/2017/02/16/%E6%B5%85%E8%81%8AHashMap%E4%B8%AD%E7%9A%84hash%E7%AE%97%E6%B3%95/)