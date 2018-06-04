# 客户端负载均衡

参考：[https://mp.weixin.qq.com/s/k4LtO3W6FcNmZU9zBmpojg](https://mp.weixin.qq.com/s/k4LtO3W6FcNmZU9zBmpojg)

我们之前有一篇文章详述了如何使用nginx实现负载均衡\([Nginx+Tomcat搭建集群，Spring Session+Redis实现Session共享](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483757&idx=1&sn=32801f98320d503360806bdd2c26e689&scene=21#wechat_redirect)\)，在这篇文章中，我们实现了如何将客户端发来的请求通过Nginx负载均衡服务器发送到不同的上游服务器去处理，这种负载均衡就是一种典型的服务端负载均衡，那么客户端负载均衡是什么?它和服务端负载均衡有什么区别?

本文是Spring Cloud系列的第五篇文章，了解前四篇文章的内容有助于更好的理解本文：

1.[使用Spring Cloud搭建服务注册中心](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483878&idx=1&sn=d49f2eb61bada3d34443a0a4017a7b72&scene=21#wechat_redirect)  
2.[使用Spring Cloud搭建高可用服务注册中心](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483893&idx=1&sn=10e897f3a1b17a8a185f08e03f2942e1&scene=21#wechat_redirect)  
3.[Spring Cloud中服务的发现与消费](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483914&idx=1&sn=f984275923667e16b996866df0cadba1&scene=21#wechat_redirect)  
4.[Eureka中的核心概念](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483926&idx=1&sn=bff01957214ef3042467a6682cb8af36&scene=21#wechat_redirect)  

### 服务端负载均衡

负载均衡是我们处理高并发、缓解网络压力和进行服务端扩容的重要手段之一，但是一般情况下我们所说的负载均衡通常都是指服务端负载均衡，服务端负载均衡又分为两种，一种是硬件负载均衡，还有一种是软件负载均衡。

硬件负载均衡主要通过在服务器节点之间安装专门用于负载均衡的设备，常见的如F5。

软件负载均衡则主要是在服务器上安装一些具有负载均衡功能的软件来完成请求分发进而实现负载均衡，常见的就是Nginx。

无论是硬件负载均衡还是软件负载均衡，它的工作原理都不外乎下面这张图：

![](https://mmbiz.qpic.cn/mmbiz_png/GvtDGKK4uYnz1yEYxVowxbEPolm3lCbe8uY5ZFp0r5DTncPjVw4k4RMMguPSM6JV12JLjRbUxclLyRqOU5d7dg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

无论是硬件负载均衡还是软件负载均衡都会维护一个可用的服务端清单，然后通过心跳机制来删除故障的服务端节点以保证清单中都是可以正常访问的服务端节点，此时当客户端的请求到达负载均衡服务器时，负载均衡服务器按照某种配置好的规则从可用服务端清单中选出一台服务器去处理客户端的请求。这就是服务端负载均衡。

### 客户端负载均衡

我们在[Spring Cloud中服务的发现与消费](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483914&idx=1&sn=f984275923667e16b996866df0cadba1&scene=21#wechat_redirect)一文中涉及到了客户端负载均衡，在那篇文章中我们提到

> “Ribbo是一个基于HTTP和TCP的客户端负载均衡器，当我们将Ribbon和Eureka一起使用时，Ribbon会从Eureka注册中心去获取服务端列表，然后进行轮询访问以到达负载均衡的作用，客户端负载均衡中也需要心跳机制去维护服务端清单的有效性，当然这个过程需要配合服务注册中心一起完成。”

从上面的描述我们可以看出，客户端负载均衡和服务端负载均衡最大的区别在于服务清单所存储的位置。在客户端负载均衡中，所有的客户端节点都有一份自己要访问的服务端清单，这些清单统统都是从Eureka服务注册中心获取的。在Spring Cloud中我们如果想要使用客户端负载均衡，方法很简单，开启`@LoadBalanced`注解即可，这样客户端在发起请求的时候会先自行选择一个服务端，向该服务端发起请求，从而实现负载均衡。具体小伙伴们可以参考[Spring Cloud中服务的发现与消费](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483914&idx=1&sn=f984275923667e16b996866df0cadba1&scene=21#wechat_redirect)这篇文章。

