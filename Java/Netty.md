# Netty

Netty是什么？

* 异步事件驱动框架，用于快速开发高性能服务端和客户端。
* 封装了JDK底层BIO，NIO模型，提供了高可用的API。
* 自带编解码器解决拆包粘包问题，用户只用关心数据逻辑。
* 精心设计的reactor线程模型支持高并发海量连接。
* 自带各种协议栈。

## 默认情况下，Netty服务端起多少线程？何时启动？

2倍的CPU核数

## Netty是如何解决JDK空轮讯的bug的？

通过计数方法

## Netty如何保证异步串行无锁化？

## Netty是在哪里检测到有新链接接入的？

## 新连接是怎样注册到NioEventLoop线程的？


## Netty是如何判断ChannelHandler类型的？

## 对于ChannelHandler的添加应该遵循什么样的顺序？

## 用户手动触发事件传播，不同的触发方式有什么区别？

## 如何把对象变成字节流，最终写到socket底层？

## 解码器抽象的解码过程

## netty里有哪些开箱即用的解码器

FastThreadLocal

Recycler

---

[Netty资料收集与整理](https://emacsist.github.io/2018/04/24/netty%E8%B5%84%E6%96%99%E6%94%B6%E9%9B%86%E4%B8%8E%E6%95%B4%E7%90%86/)


[翻译-Netty中的引用计数对象](https://emacsist.github.io/2018/04/28/%E7%BF%BB%E8%AF%91netty%E4%B8%AD%E7%9A%84%E5%BC%95%E7%94%A8%E8%AE%A1%E6%95%B0%E5%AF%B9%E8%B1%A1/)
