# Concurrency
## 26 Concurrency and Threads code
多线程的程序分享相同的地址空间,因此可以访问相同的数据.

多线程运行在一个处理器上,**context switch**一定会发生作用.

> any stack-allocated variables, parameters, return
values, and other things that we put on the stack will be placed in
what is sometimes called **thread-local** storage, i.e., the stack of the relevant
thread.即每个线程都会有各自的Stack.

### 26.1 Why Use Threads?
我们使用线程,最少有两个主要原因:
* parallelism
*  to avoid blocking program
progress due to slow I/O

### 26.2 An Example: Thread Creation
> What runs next is determined by the OS
scheduler, and although the scheduler likely implements some sensible
algorithm, it is hard to know what will run at any given moment in time.

### 26.3 Why It Gets Worse: Shared Data
> The simple thread example we showed above was useful in showing
how threads are created and how they can run in different orders depending
on how the scheduler decides to run them. What it doesn’t show you,
though, is how threads interact when they access shared data.

### 26.4 The Heart Of The Problem: Uncontrolled Scheduling
### 26.5 The Wish For Atomicity
原子性
### 26.6 One More Problem: Waiting For Another
> where one thread must
wait for another to complete some action before it continues

这又给我们的并发带来了问题.

### 