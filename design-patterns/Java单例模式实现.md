# Java单例模式实现
优势 : 单例模式只生成一个实例, 减少了系统性能开销,节省内存，加快对象访问速度

## 适用场景
* 需要频繁实例化然后销毁的对象
* 创建对象时耗时过多或者耗资源过多，但又经常用到的对象
* 有状态的工具类对象
* 频繁访问数据库或文件的对象

## 应用场景
* Windows中的任务管理器
* 文件系统, 一个操作系统只能有一个文件系统
* 数据库连接池的设计与实现
* Spring中, 一个Component就只有一个实例Java-Web中, 一个Servlet类只有一个实例
* 日志系统,应用配置

## 实现要点
* 声明为private来隐藏构造器
* private static Singleton实例
* 声明为public来暴露实例获取方法(必须是静态方法，通常使用getInstance这个名称)

## 实现方式

### 懒汉式，线程不安全的实现
```Java
public class Singleton {
    private static Singleton instance;
    private Singleton() {
    };
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}  
```

优势是，实现了懒加载，在需要时才构造实例．

缺点是，没有考虑到线程安全，可能存在多个访问者同时访问，并同时构造了多个对象的问题．

### 懒汉式，线程安全但效率低下的实现
```Java
public class Singleton {
    private static Singleton instance;
    private Singleton() {
    };
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}  
```

由于对象只需要在初次初始化时需要同步，多数情况下不需要互斥的获得对象，加锁会造成巨大无意义的资源消耗．

### 饿汉式
```Java
public class Singleton {
    private static final Singleton instance = new Singleton();
    private Singleton() {
    };
    public static Singleton getInstance() {
        return instance;
    }
}  
```

instance在类加载时进行初始化，避免了同步问题。

饿汉式的优势在于实现简单，劣势在于不是懒加载，在单例较多的情况下，会造成内存占用，加载速度慢问题．

### 静态内部类
```Java
public class Singleton {
    private Singleton() {
    };
    public static Singleton getInstance() {
        return Holder.instance;
    }
    private static class Holder{
        private static Singleton instance = new Singleton();
    }
}  
```

由于内部类不会在类的外部被使用，所以只有在调用getInstance()方法时才会被加载。同时依赖JVM的ClassLoader类加载机制保证了不会出现同步问题。

### 枚举方法

```Java
public enum Singleton {
    INSTANCE;
    public void otherMethods(){
        System.out.println("Something");
    }
}
```

Effective Java作者Josh Bloch 提倡的方式，解决了以下三个问题：

* 自由序列化
* 保证只有一个实例
* 线程安全

如果我们想调用它的方法时，仅需要以下操作：
```Java
public class Hello {
    public static void main(String[] args){
        Singleton.INSTANCE.otherMethods();
    }
}
```

### 双重检测
```Java
public class Singleton {
    private static volatile Singleton instance;
    private Singleton() {
    };
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}  
```
