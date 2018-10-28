# Netty

参考：

[https://waylau.com/netty-4-user-guide/](https://waylau.com/netty-4-user-guide/)

netty 聊天室demo ：[https://github.com/waylau/netty-4-user-guide-demos](https://github.com/waylau/netty-4-user-guide-demos)

Netty相当于是NIO的封装，提供标准的API，方便开发者。



Netty 有一个简单却不失强大的架构。这个架构由三部分组成——缓冲（buffer），通道（channel），事件模型（event model）——所有的高级特性都构建在这三个核心组件之上。一旦你理解了它们之间的工作原理，你便不难理解在本章简要提及的更多高级特性。



Netty 使用自建的 buffer API，而不是使用 NIO 的 [ByteBuffer](http://docs.oracle.com/javase/7/docs/api/java/nio/ByteBuffer.html?is-external=true) 来表示一个连续的字节序列。与 ByteBuffer 相比这种方式拥有明显的优势。Netty 使用新的 buffer 类型 [ByteBuf](http://netty.io/4.0/api/io/netty/buffer/ByteBuf.html)，被设计为一个可从底层解决 ByteBuffer 问题，并可满足日常网络应用开发需要的缓冲类型。





### HTTP 实现 <a id="http-&#x5B9E;&#x73B0;"></a>

HTTP无 疑是互联网上最受欢迎的协议，并且已经有了一些例如 Servlet 容器这样的 HTTP 实现。因此，为什么 Netty 还要在其核心模块之上构建一套 HTTP 实现？

与现有的 HTTP 实现相比 Netty 的 HTTP 实现是相当与众不同的。在HTTP 消息的低层交互过程中你将拥有绝对的控制力。这是因为 Netty 的HTTP 实现只是一些 HTTP codec 和 HTTP 消息类的简单组合，这里不存在任何限制——例如那种被迫选择的线程模型。你可以随心所欲的编写那种可以完全按照你期望的工作方式工作的客户端或服务器端代码。这包括线程模型，连接生命期，快编码，以及所有 HTTP 协议允许你做的，所有的一切，你都将拥有绝对的控制力。

由于这种高度可定制化的特性，你可以开发一个非常高效的HTTP服务器，例如：

* 要求持久化链接以及服务器端推送技术的聊天服务（如，[Comet](https://en.wikipedia.org/wiki/Comet_%28programming%29) \)
* 需要保持链接直至整个文件下载完成的媒体流服务（如，2小时长的电影）
* 需要上传大文件并且没有内存压力的文件服务（如，上传1GB文件的请求）
* 支持大规模混合客户端应用用于连接以万计的第三方异步 web 服务。

