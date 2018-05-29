# Kafka vs RabbitMQ vs RocketMQ

参考：

{% embed data="{\"url\":\"http://jm.taobao.org/2016/04/01/kafka-vs-rabbitmq-vs-rocketmq-message-send-performance/\",\"type\":\"link\",\"title\":\"Kafka、RabbitMQ、RocketMQ消息中间件的对比 —— 消息发送性能\",\"description\":\"引言分布式系统中,我们广泛运用消息中间件进行系统间的数据交换,便于异步解耦。现在开源的消息中间件有很多,前段时间我们自家的产品 RocketMQ \(MetaQ的内核\) 也顺利开源,得到大家的关注。 那么,消息中间件性能究竟哪家强? 带着这个疑问,我们中间件测试组对常见的三类消息产品\(Kafka、RabbitMQ、RocketMQ\)做了性能比较。 Kafka是LinkedIn开源的分布式发布-订阅消\",\"icon\":{\"type\":\"icon\",\"url\":\"http://jm.taobao.org/favicon.ico?v=5.0.1\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"http://img3.tbcdn.cn/5476e8b07b923/TB1GzuAOFXXXXXGXFXXXXXXXXXX\",\"width\":594,\"height\":151,\"aspectRatio\":0.2542087542087542}}" %}



Kafka是LinkedIn开源的分布式发布-订阅消息系统，目前归属于Apache定级项目。Kafka主要特点是基于Pull的模式来处理消息消费，追求高吞吐量，一开始的目的就是用于日志收集和传输。0.8版本开始支持复制，不支持事务，对消息的重复、丢失、错误没有严格要求，适合产生大量数据的互联网服务的数据收集业务。

RabbitMQ是使用Erlang语言开发的开源消息队列系统，基于AMQP协议来实现。AMQP的主要特征是面向消息、队列、路由（包括点对点和发布/订阅）、可靠性、安全。AMQP协议更多用在企业系统内，对数据一致性、稳定性和可靠性要求很高的场景，对性能和吞吐量的要求还在其次。

RocketMQ是阿里开源的消息中间件，它是纯Java开发，具有高吞吐量、高可用性、适合大规模分布式系统应用的特点。RocketMQ思路起源于Kafka，但并不是Kafka的一个Copy，它对消息的可靠传输及事务性做了优化，目前在阿里集团被广泛应用于交易、充值、流计算、消息推送、日志流式处理、binglog分发等场景。





