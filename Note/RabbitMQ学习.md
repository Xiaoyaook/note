# RabbitMQ学习

## 消息应答：使用ack确认Message的正确传递

执行一个任务可能需要花费几秒钟，你可能会担心如果一个消费者在执行任务过程中挂掉了。一旦RabbitMQ将消息分发给了消费者，就会从内存中删除。在这种情况下，如果正在执行任务的消费者宕机，会丢失正在处理的消息和分发给这个消费者但尚未处理的消息。

为了确保消息不会丢失，RabbitMQ支持消息应答。消费者发送一个消息应答，告诉RabbitMQ这个消息已经接收并且处理完毕了。RabbitMQ就可以删除它了。

每个Message都要被acknowledged（确认，ack）。我们可以显示的在程序中去ack，也可以自动的ack。如果有数据没有被ack，那么RabbitMQ Server会把这个信息发送到下一个Consumer。

消息应答是默认打开的。我们通过显示的设置autoAsk=true关闭这种机制。现即自动应答开，一旦我们完成任务，消费者会自动发送应答。通知RabbitMQ消息已被处理，可以从内存删除。如果消费者因宕机或链接失败等原因没有发送ACK（不同于ActiveMQ，在RabbitMQ里，消息没有过期的概念），则RabbitMQ会将消息重新发送给其他监听在队列的下一个消费者。

## Message durability（消息持久化）

原生的 RabbitMQ 客户端需要完成三个步骤：
* 交换器的持久化
* 队列的持久化
* 消息的持久化

Spring AMQP 是对原生的 RabbitMQ 客户端的封装。一般情况下，我们只需要定义交换器的持久化和队列的持久化。

交换器的持久化配置如下:
```
// 参数1 name ：交互器名
// 参数2 durable ：是否持久化
// 参数3 autoDelete ：当所有消费客户端连接断开后，是否自动删除队列
new TopicExchange(name, durable, autoDelete)
```
配置队列的持久化:
```
// 参数1 name ：队列名
// 参数2 durable ：是否持久化
// 参数3 exclusive ：仅创建者可以使用的私有队列，断开后自动删除
// 参数4 autoDelete : 当所有消费客户端连接断开后，是否自动删除队列
new Queue(name, durable, exclusive, autoDelete);
```
---
参考博客: [RabbitMQ 的消息持久化与 Spring AMQP 的实现剖析](http://blog.720ui.com/2017/rabbitmq_action_durable/)
