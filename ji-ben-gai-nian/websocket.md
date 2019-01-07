# Websocket

参考：

{% embed url="https://zh.wikipedia.org/wiki/WebSocket" %}

{% embed url="https://www.zhihu.com/question/20215561" %}





WebSocket是类似Socket的TCP长连接通讯模式。一旦WebSocket连接建立后，后续数据都以帧序列的形式传输。在客户端断开WebSocket连接或Server端中断连接前，不需要客户端和服务端重新发起连接请求。在海量并发及客户端与服务器交互负载流量大的情况下，极大的节省了网络带宽资源的消耗，有明显的性能优势，且客户端发送和接受消息是在同一个持久连接上发起，实时性优势明显。

相比HTTP长连接，WebSocket有以下特点：

* 是真正的全双工方式，建立连接后客户端与服务器端是完全平等的，可以互相主动请求。而HTTP长连接基于HTTP，是传统的客户端对服务器发起请求的模式。
* HTTP长连接中，每次数据交换除了真正的数据部分外，服务器和客户端还要大量交换HTTP header，信息交换效率很低。Websocket协议通过第一个request建立了TCP连接之后，之后交换的数据都不需要发送 HTTP header就能交换数据，这显然和原有的HTTP协议有区别所以它需要对服务器和客户端都进行升级才能实现（主流浏览器都已支持HTML5）。此外还有 multiplexing、不同的URL可以复用同一个WebSocket连接等功能。这些都是HTTP长连接不能做到的。



![WebSocket &#x804A;&#x5929;&#x5BA4; demo](../.gitbook/assets/image%20%288%29.png)



![&#x5FAE;&#x4FE1;web&#x7248;&#x672C;&#x7528;&#x7684;&#x662F;&#x957F;&#x8FDE;&#x63A5;](../.gitbook/assets/image%20%2814%29.png)

注：

单工：简单的说就是一方只能发信息，另一方则只能收信息，通信是单向的。

     半双工：比单工先进一点，就是双方都能发信息，但同一时间则只能一方发信息。

     全双工：比半双工再先进一点，就是双方不仅都能发信息，而且能够同时发送。

