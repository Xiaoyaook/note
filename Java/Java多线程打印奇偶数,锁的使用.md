# Java多线程打印奇偶数

> 2个多线程交替打印1-100内数字,其中t1线程打印奇数,t2程打印偶数 

主要考察对多线程创建以及多线程执行顺序的应用,难点是通过对一个对象的加锁,避免多线程随机打印,用一个开关控制打印奇数还是偶数 

## 第一个方法

使用ReentrantLock

```Java
public class TwoThread {

    private int start = 1;

    /**
     * 保证内存可见性
     * 其实用锁了之后也可以保证可见性 这里用不用 volatile 都一样
     */
    private boolean flag = false;

    /**
     * 重入锁
     */
    private final static Lock LOCK = new ReentrantLock();

    public static void main(String[] args) {
        TwoThread twoThread = new TwoThread();

        Thread t1 = new Thread(new OuNum(twoThread));
        t1.setName("t1");


        Thread t2 = new Thread(new JiNum(twoThread));
        t2.setName("t2");

        t1.start();
        t2.start();
    }

    /**
     * 偶数线程
     */
    public static class OuNum implements Runnable {

        private TwoThread number;

        public OuNum(TwoThread number) {
            this.number = number;
        }

        @Override
        public void run() {
            while (number.start <= 100) {

                if (number.flag) {
                    try {
                        LOCK.lock();
                        System.out.println(Thread.currentThread().getName() + "+-+" + number.start);
                        number.start++;
                        number.flag = false;


                    } finally {
                        LOCK.unlock();
                    }
                } else {
                    try {
                        //防止线程空转
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }

    /**
     * 奇数线程
     */
    public static class JiNum implements Runnable {

        private TwoThread number;

        public JiNum(TwoThread number) {
            this.number = number;
        }

        @Override
        public void run() {
            while (number.start <= 100) {

                if (!number.flag) {
                    try {
                        LOCK.lock();
                        System.out.println(Thread.currentThread().getName() + "+-+" + number.start);
                        number.start++;
                        number.flag = true;


                    } finally {
                        LOCK.unlock();
                    }
                } else {
                    try {
                        //防止线程空转
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
}
```

## 第二个方法

使用内置锁synchronized，synchronized也是个可重入锁

```Java
public class ThreadTest {

    public boolean flag;

    public class JiClass implements Runnable {
        public ThreadTest t;

        public JiClass(ThreadTest t) {
            this.t = t;
        }

        @Override
        public void run() {
            int i = 1;  //本线程打印奇数,则从1开始
            while (i < 100) {
                //两个线程的锁的对象只能是同一个object
                synchronized (t) {
                    if (!t.flag) {
                        System.out.println("-----" + i);

                        i += 2;
                        t.flag = true;
                        t.notify();

                    } else {
                        try {
                            t.wait();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }

                }
            }
        }
    }


    public class OuClass implements Runnable {
        public ThreadTest t;

        public OuClass(ThreadTest t) {
            this.t = t;
        }

        @Override
        public void run() {
            int i = 2;//本线程打印偶数,则从2开始
            while (i <= 100)
                //两个线程的锁的对象只能是同一个object
                synchronized (t) {
                    if (t.flag) {
                        System.out.println("-----------" + i);
                        i += 2;
                        t.flag = false;
                        t.notify();

                    } else {
                        try {
                            t.wait();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                }
        }
    }


    public static void main(String[] args) {
        ThreadTest tt = new ThreadTest();
        JiClass jiClass = tt.new JiClass(tt);
        OuClass ouClass = tt.new OuClass(tt);
        new Thread(jiClass).start();
        new Thread(ouClass).start();
    }
}
```

## Synchronized和重入锁ReentrantLock异同点

### 相同点

它们都是加锁方式同步，而且都是阻塞式的同步，也就是说当如果一个线程获得了对象锁，进入了同步块，其他访问该同步块的线程都必须阻塞在同步块外面等待，而进行线程阻塞和唤醒的代价是比较高的（操作系统需要在用户态与内核态之间来回切换，代价很高，不过可以通过对锁优化进行改善）。

### 区别

这两种方式最大区别就是对于Synchronized来说，它是java语言的关键字，是原生语法层面的互斥，需要jvm实现。而ReentrantLock它是JDK 1.5之后提供的API层面的互斥锁，需要lock()和unlock()方法配合try/finally语句块来完成。

### Synchronized

Synchronized进过编译，会在同步块的前后分别形成**monitorenter**和**monitorexit**这个两个字节码指令。在执行monitorenter指令时，首先要尝试获取对象锁。如果这个对象没被锁定，或者当前线程已经拥有了那个对象锁，把锁的计算器加1，相应的，在执行monitorexit指令时会将锁计算器就减1，当计算器为0时，锁就被释放了。如果获取对象锁失败，那当前线程就要阻塞，直到对象锁被另一个线程释放为止。

### ReentrantLock

由于ReentrantLock是java.util.concurrent包下提供的一套互斥锁，相比Synchronized，ReentrantLock类提供了一些高级功能，主要有以下3项：

1. 等待可中断，持有锁的线程长期不释放的时候，正在等待的线程可以选择放弃等待，这相当于Synchronized来说可以避免出现死锁的情况。
2. 公平锁，多个线程等待同一个锁时，必须按照申请锁的时间顺序获得锁，Synchronized锁非公平锁，ReentrantLock默认的构造函数是创建的非公平锁，可以通过参数true设为公平锁，但公平锁表现的性能不是很好。
3. 锁绑定多个条件，一个ReentrantLock对象可以同时绑定对个对象。

## synchronized作用在方法上和代码块区别
synchronized方法锁住的是this，
synchronized(this){ }，锁住this，只要多个线程同时访问同一个SynchronizedTest实例（this），就会发生不能同时访问此代码块。
synchronized(Object){ }，锁住Object，只要多个线程同时访问同一个Object就会发生不能同时访问此代码块。

同步的目的就避免操作同一个数据，所以操作不同数据就不必要同步。

理解下面代码的区别，就真正理解了同步：
```Java
public class SynchronizedTest{  
      
    Person p = new Person("lin", 15);  
      
    public synchronized void say(){//相当于锁住this，效果和say3()一样，只要多个线程同时访问同一个SynchronizedTest实例（相当于this），就会发生不能同时访问此方法  
        System.out.println(p.getName());  
    }  
      
    public void say2(){  
        synchronized(p){//相当于锁住p，只要多个线程同时访问此代码块且是同一个p，就会发生不能同时访问此代码块锁住  
            System.out.println(p.getName());  
        }  
    }  
      
    public void say3(){  
        synchronized(this){//锁住this，效果和say()一样，只要多个线程同时访问同一个SynchronizedTest实例（this），就会发生不能同时访问此代码块  
            System.out.println(p.getName());  
        }  
    }  
}  
```

## synchronized(this)和synchronized(Xx.class)区别

synchronized应用在static方法上，那是对当前对应的*.Class进行持锁。

同步synchronized(*.class)代码块的作用其实和synchronized static方法作用一样。Class锁对类的所有对象实例起作用。

## synchronized和lock的用法区别

synchronized：在需要同步的对象中加入此控制，synchronized可以加在方法上，也可以加在特定代码块中，括号中表示需要锁的对象。
 
lock：需要显示指定起始位置和终止位置。一般使用ReentrantLock类做为锁，多个线程中必须要使用一个ReentrantLock类做为对象才能保证锁的生效。且在加锁和解锁处需要通过lock()和unlock()显示指出。所以一般会在finally块中写unlock()以防死锁。