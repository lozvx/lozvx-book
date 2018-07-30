# RabbitMQ

{% embed data="{\"url\":\"http://www.rabbitmq.com/getstarted.html\",\"type\":\"link\",\"title\":\"RabbitMQ - Getting started with RabbitMQ\"}" %}

[https://blog.csdn.net/column/details/rabbitmq.html](https://blog.csdn.net/column/details/rabbitmq.html)

[https://blog.csdn.net/column/details/slimina-rabbitmq.html](https://blog.csdn.net/column/details/slimina-rabbitmq.html)

  
A _producer_ is a user application that sends messages.

A _queue_ is a buffer that stores messages.

A _consumer_ is a user application that receives messages.



三个概念：

 **Exchanges** are where producers publish their messages.

 **Queues**are where the messages end up and are received by consumers

 **Bindings** are how the messages get routed from the exchange to particular queues.

RabbitMQ的消息模型是生产者不会直接发送消息到Queue。事实上，生产者都不会知道消息会被传递到Queue。相反，生产者只会发送消息到Exchange。Exchange是个 很简单的东西，一方面，它从生产者接受消息，另一方面，它推送消息到Queue。Exchange必须清楚的知道怎么处理收到的消息。是发送到一个Queue还是多个Queues，还是直接丢掉。这些规则都是由Exchange Type定义。

> The core idea in the messaging model in RabbitMQ is that the producer never sends any messages directly to a queue. Actually, quite often the producer doesn't even know if a message will be delivered to any queue at all.
>
> Instead, the producer can only send messages to an _exchange_. An exchange is a very simple thing. On one side it receives messages from producers and the other side it pushes them to queues. The exchange must know exactly what to do with a message it receives. Should it be appended to a particular queue? Should it be appended to many queues? Or should it get discarded. The rules for that are defined by the _exchange type_.

队列类型：

1.简单队列 Simple

2.工作队列Work queue 

一对多

3.订阅消费者模式

Sending messages to many consumers at once

  
4.路由模式Routing

5.Topics

* \* \(star\) can substitute for exactly one word.
* \# \(hash\) can substitute for zero or more words.

6.RPC



