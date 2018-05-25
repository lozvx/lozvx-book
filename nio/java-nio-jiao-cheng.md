# Java NIO 教程

### Java NIO: Channels and Buffers {#ba20c0f0008c08cf086097e48e0c3d7c}

标准的IO编程接口是面向字节流和字符流的。而NIO是面向**通道和缓冲区**的，数据总是从通道中读到buffer缓冲区内，或者从buffer写入到通道中。

### Java NIO: Non-blocking IO {#80bfca892bff24a758076058048c17b1}

Java NIO使我们可以进行非阻塞IO操作。比如说，单线程中从通道读取数据到buffer，同时可以继续做别的事情，当数据读取到buffer中后，线程再继续处理数据。写数据也是一样的。

### Java NIO: Selectors {#d7e147e0d366f32ea9bf0348e89e51e9}

NIO中有一个“selectors”的概念。selector可以检测多个通道的事件状态（例如：链接打开，数据到达）这样单线程就可以操作多个通道的数据。 所有这些都会在后续章节中更详细的介绍。



1.  Channel

* FileChannel
* DatagramChannel
* SocketChannel
* ServerSocketChannel

2. Buffer

* `ByteBuffer`
* `MappedByteBuffer`
* `CharBuffer`
* `DoubleBuffer`
* `FloatBuffer`
* `IntBuffer`
* `LongBuffer`
* `ShortBuffer`

缓冲区其实就是一块内存区域，可以写入数据，之后再读取数据。只不过这块内存区域被包装成一个NIO的缓冲区对象，并提供了一些方法以便于对其进行操作。  


3. Java NIO Scatter（分散）、Gather（聚集）



```java
ByteBuffer header = ByteBuffer.allocate(128);
ByteBuffer body   = ByteBuffer.allocate(1024);
ByteBuffer[] bufferArray = { header, body };
channel.read(bufferArray);
```

```text

ByteBuffer header = ByteBuffer.allocate(128);ByteBuffer body   = ByteBuffer.allocate(1024);//把数据写入各个缓冲区中ByteBuffer[] bufferArray = { header, body };channel.write(bufferArray);
```

#### FileChannel {#filechannel}

Java NIO的`FileChannel`是连接到文件的通道，可以通过它读写文件。Java NIO的`FileChannel`可以替代标准的Java IO API来读写文件。

#### SocketChannel {#socketchannel}

Java NIO `SocketChannel` 是连接到 **TCP** 网络套接字的一种通道。它其实就是Java NIO中的[标准Java网络Socket](http://docs.oracle.com/javase/8/docs/api/java/net/Socket.html)。有两种创建方法：

1. 打开`SocketChannel`并连接到网络中的某个服务器
2. 在`ServerSocketChannel`中当有连接进来时，可以创建一个`SocketChannel`

**阻塞型IO管道的缺点**

虽然阻塞型IO管道易于实现，不幸的是它的缺点，就是每个分解成消息的流都需要一个独立的线程来处理。必须这样做的原因是每个流的IO接口都会被阻塞，直到有数据读出为止。这就意味着单个线程不能够先尝试去读取一个流，当没有数据时再尝试去读取下一个流。只要这个线程去读取某个流，它就会被阻塞，直到有数据读出为止，该线程才能继续工作。

如果IO管道是处理很多并发连接的服务器的一部分，那么服务器就需要为每个活动的连接来创建一个线程。在只有上百个并发量的情况下，任何时候服务器可能都不会产生问题。但是，在百万级别的并发量下，这种设计可能就不能很灵活的应对了。每个线程会占用320K（32位的JVM）到1024K（64为的JVM）的内存，所以一百万个线程会占用1TB的内存！这还仅仅是在服务器处理任何传入消息之前（要知道，服务器在处理各种传入的消息时还会产生很多对象，还需要更多的内存）。

为了减少或者保持线程数，很多服务器都设计为使用一个线程池（比如100个线程）每次从到达的连接中读取消息。到达的连接会组成一个队列，线程池中的线程会依次、分批处理这个队列中的连接。如下图：  


