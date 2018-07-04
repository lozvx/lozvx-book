# NIO

参考：

谷歌搜索排名第一的Java NIO教程 ：[http://tutorials.jenkov.com/java-nio/index.html](http://tutorials.jenkov.com/java-nio/index.html)

译文： ​[http://wiki.jikexueyuan.com/project/java-nio-zh/java-nio-tutorial.html](http://wiki.jikexueyuan.com/project/java-nio-zh/java-nio-tutorial.html)

[http://xintq.net/2017/06/12/everything-about-java-nio/](http://xintq.net/2017/06/12/everything-about-java-nio/)

知乎提问：[https://www.zhihu.com/question/29005375](https://www.zhihu.com/question/29005375)

美团技术: [https://tech.meituan.com/nio.html](https://tech.meituan.com/nio.html)



NIO（Non-blocking I/O，在Java领域，也称为New I/O），是一种同步非阻塞的I/O模型，也是I/O多路复用的基础，已经被越来越多地应用到大型应用服务器，成为解决高并发与大量连接、I/O处理问题的有效方式。  




![](../.gitbook/assets/image%20%282%29.png)



  
常见的客户端BIO+连接池模型，可以建立n个连接，然后当某一个连接被I/O占用的时候，可以使用其他连接来提高性能。

但多线程的模型面临和服务端相同的问题：如果指望增加连接数来提高性能，则连接数又受制于线程数、线程很贵、无法建立很多线程，则性能遇到瓶颈。



### 四种模型

1. 同步阻塞（Blocking IO）：最简单的一种IO模型，用户线程在进行IO操作的时候通常是个系统调用，用户线程会由用户空间进入内核空间，内核空间数据包准备好后会将数据拷贝到用户空间，这个时候线程在用户态继续执行。 
2. 同步非阻塞（Non-blocking IO）：同步非阻塞IO即在同步阻塞的基础之上将socket设置为NONBLOCK。这样用户线程在发起IO操作之后可以立即返回，但是用户线程需要不断轮询来请求数据。 
3. IO多路复用（IO Multiplexing）：即Reactor设计模式，多路复用模型从流程上和同步阻塞的区别不大，主要区别在于操作系统为用户提供了同时轮询多个IO句柄来查看是否有IO事件的接口（如select），这从根本上允许用户可以使用单个线程来管理多个IO句柄的问题。 
4. 异步IO（Asynchronous IO）：即Proactor设计模式。在异步IO模型中，用户不需要去轮询IO事件，然后才进行数据的读取，处理；在异步IO模型中，IO事件就绪的时候，内核会开启一个独立的内核线程去执行执行IO操作，实现真正的异步IO。这个时候用户线程可以直接读取内核线程准备好的数据。

NIO框架 netty mina

[https://github.com/waylau/netty-4-user-guide](https://github.com/waylau/netty-4-user-guide)

[https://github.com/waylau/apache-mina-2.x-user-guide](https://github.com/waylau/apache-mina-2.x-user-guide)



