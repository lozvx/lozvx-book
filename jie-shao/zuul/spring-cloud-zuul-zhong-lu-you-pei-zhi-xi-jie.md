# Spring Cloud Zuul中路由配置细节

{% embed data="{\"url\":\"https://mp.weixin.qq.com/s/FsvZgkvpI0S6rposacGiiQ\",\"type\":\"link\",\"icon\":{\"type\":\"icon\",\"url\":\"https://res.wx.qq.com/mmbizwap/en\_US/htmledition/images/icon/common/favicon22c41c.ico\",\"aspectRatio\":0}}" %}



上篇文章我们介绍了API网关的基本构建方式以及请求过滤，小伙伴们对Zuul的作用应该已经有了一个基本的认识，但是对于路由的配置我们只是做了一个简单的介绍，本文我们就来看看路由配置的其他一些细节。

本文是Spring Cloud系列的第二十篇文章，了解前十九篇文章内容有助于更好的理解本文：

1.[使用Spring Cloud搭建服务注册中心](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483878&idx=1&sn=d49f2eb61bada3d34443a0a4017a7b72&scene=21#wechat_redirect)  
2.[使用Spring Cloud搭建高可用服务注册中心](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483893&idx=1&sn=10e897f3a1b17a8a185f08e03f2942e1&scene=21#wechat_redirect)  
3.[Spring Cloud中服务的发现与消费](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483914&idx=1&sn=f984275923667e16b996866df0cadba1&scene=21#wechat_redirect)  
4.[Eureka中的核心概念](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483926&idx=1&sn=bff01957214ef3042467a6682cb8af36&scene=21#wechat_redirect)  
5.[什么是客户端负载均衡](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483930&idx=1&sn=637efb117ab87f9cfb629ae624c0a064&scene=21#wechat_redirect)  
6.[Spring RestTemplate中几种常见的请求方式](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483941&idx=1&sn=6a86e429f704e900987a470709a94159&scene=21#wechat_redirect)  
7.[RestTemplate的逆袭之路，从发送请求到负载均衡](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483946&idx=1&sn=6bbeb819fdc1d5528e6e4e8deebb5234&scene=21#wechat_redirect)  
8.[Spring Cloud中负载均衡器概览](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483950&idx=1&sn=26f53e77ddf6276943e884f87753a492&scene=21#wechat_redirect)  
9.[Spring Cloud中的负载均衡策略](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483955&idx=1&sn=2f5e5c93a0a1af5db1a4604b7d72f1c1&scene=21#wechat_redirect)  
10.[Spring Cloud中的断路器Hystrix](http://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483966&idx=1&sn=b8cee7f1b54e5ecff13acc87e4b0489c&scene=21#wechat_redirect)  
11.[Spring Cloud自定义Hystrix请求命令](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483976&idx=1&sn=3a156d6231635ccfceeb8a54dda7a1ea&scene=21#wechat_redirect)  
12.[Spring Cloud中Hystrix的服务降级与异常处理](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483980&idx=1&sn=1c24c67762afcdef2fd6f3c2a2baa27d&scene=21#wechat_redirect)  
13.[Spring Cloud中Hystrix的请求缓存](http://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247483986&idx=1&sn=b5fa246b2f213881b712d29e8276e6b2&scene=21#wechat_redirect)  
14.[Spring Cloud中Hystrix的请求合并](http://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247484032&idx=1&sn=cc620d664443795115f0410ca03a1164&scene=21#wechat_redirect)  
15.[Spring Cloud中Hystrix仪表盘与Turbine集群监控](http://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247484036&idx=1&sn=e34e995aed9f43c21a5ef88d852c9c73&scene=21#wechat_redirect)  
16.[Spring Cloud中声明式服务调用Feign](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247484040&idx=1&sn=aae350e6dc1a9f42882c9b316cf33390&scene=21#wechat_redirect)  
17.[Spring Cloud中Feign的继承特性](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247484045&idx=1&sn=5e570f269feb6ac53cd2093dc9da811a&scene=21#wechat_redirect)  
18.[Spring Cloud中Feign配置详解](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247484049&idx=1&sn=1629e565a1504477d6e3828a2991253b&scene=21#wechat_redirect)  
19.[Spring Cloud中的API网关服务Zuul](https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247484053&idx=1&sn=3a448fe45fd2616cf2e77b3a9f7bf6be&scene=21#wechat_redirect)  

首先我们来回忆一下上篇文章我们配置路由规则的那两行代码：

```text
zuul.routes.api-a.path=/api-a/**
zuul.routes.api-a.serviceId=feign-consumer
```

我们说当我的访问地址符合/api-a/\*\*规则的时候，会被自动定位到feign-consumer服务上去，不过两行代码有点麻烦，我们可以用下面一行代码来代替，如下：

```text
zuul.routes.feign-consumer=/api-a/**
```

zuul.routes后面跟着的是服务名，服务名后面跟着的是路径规则，这种配置方式显然更简单。  
如果映射规则我们什么都不写，系统也给我们提供了一套默认的配置规则，默认的配置规则如下：

```text
zuul.routes.feign-consumer.path=/feign-consumer/**
zuul.routes.feign-consumer.serviceId=feign-consumer
```

默认情况下，Eureka上所有注册的服务都会被Zuul创建映射关系来进行路由，但是对于我这里的例子来说，我希望提供服务的是feign-consumer，hello-service作为服务提供者只对服务消费者提供服务，不对外提供服务，如果使用默认的路由规则，则Zuul也会自动为hello-service创建映射规则，这个时候我们可以采用如下方式来让Zuul跳过hello-service服务，不为其创建路由规则：

```text
zuul.ignored-services=hello-service
```

有的小伙伴可能为有疑问，我们定义路由规则`/api-a/**`的时候，为什么最后面是两个\*，一个可不可以呢？当然可以，不过意义可就不一样了，Zuul中的路由匹配规则使用了Ant风格定义，一共有三种不同的通配符：

| 通配符 | 含义 | 举例 | 解释 |
| :--- | :--- | :--- | :--- |
| ? | 匹配任意单个字符 | /feign-consumer/? | 匹配/feign-consumer/a,/feign-consumer/b,/feign-consumer/c等 |
| \* | 匹配任意数量的字符 | /feign-consumer/\* | 匹配/feign-consumer/aaa,feign-consumer/bbb,/feign-consumer/ccc等，无法匹配/feign-consumer/a/b/c |
| \*\* | 匹配任意数量的字符 | /feign-consumer/\* | 匹配/feign-consumer/aaa,feign-consumer/bbb,/feign-consumer/ccc等，也可以匹配/feign-consumer/a/b/c |

有的时候我们还会遇到这样一个问题，比如我有两个服务，一个叫做feign-consumer，还有一个叫做feign-consumer-hello，此时我的路由配置规则可能这样来写：

```text
zuul.routes.feign-consumer.path=/feign-consumer/**
zuul.routes.feign-consumer.serviceId=feign-consumer

zuul.routes.feign-consumer-hello.path=/feign-consumer/hello/**
zuul.routes.feign-consumer-hello.serviceId=feign-consumer-hello
```

此时我访问feign-consumer-hello的路径会同时被这两条规则所匹配，Zuul中的路径匹配方式是一种线性匹配方式，即按照路由匹配规则的存储顺序依次匹配，因此我们只需要确保feign-consumer-hello的匹配规则被先定义feign-consumer的匹配规则被后定义即可，但是在properties文件中我们不能保证这个先后顺序，此时我们需要用YAML来配置，这个时候我们可以删掉resources文件夹下的application.properties，然后新建一个application.yml，内容如下：

```text
spring:
  application:
    name: api-gateway
server:
  port: 2006
zuul:
  routes:
    feign-consumer-hello:
      path: /feign-consumer/hello/**
      serviceId: feign-consumer-hello
    feign-consumer:
      path: /feign-consumer/**
      serviceId: feign-consumer
eureka:
  client:
    service-url:
      defaultZone: http://localhost:1111/eureka/
```

这个时候我们就可以确保先加载feign-consumer-hello的匹配规则，后加载feign-consumer的匹配规则。

上文我们说了一个zuul.ignored-services=hello-service属性可以忽略掉一个服务，不给某个服务设置映射规则，这个配置我们可以进一步细化，比如说我不想给/hello接口路由，那我们可以按如下方式配置\(后面我都用yaml配置\)：

```text
zuul:
  ignored-patterns: /**/hello/**
```

此时访问/hello接口就会报404错误，同时我们也可以看到后台打印如下日志：

![](https://mmbiz.qpic.cn/mmbiz_png/GvtDGKK4uYnG6clUSNI5WQNpdKcMPcic2YU2rFJEPUnnAiaslO1tI5A8wEu2edr6ibgoibOGo54zE5Dfu33s37ysfg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

此外，我们也可以统一的为路由规则增加前缀，设置方式如下：

```text
zuul:
  prefix: /myapi
```

此时我们的访问路径就变成了http://localhost:2006/myapi/feign-consumer/hello1。  

一般情况下API网关只是作为系统的统一入口，但是有的时候我们可能也需要在API网关上做一点业务逻辑操作，比如我现在在api-gateway项目中新建如下Controller：

```text
@RestController
public class HelloController {
    @RequestMapping("/local")
    public String hello() {
        return "hello api gateway";
    }
}
```

我希望用户在访问/local时能够自动跳转到这个方法上来处理，那么此时我们需要用到Zuul的本地跳转，配置方式如下：

```text
zuul:
  prefix: /myapi
  ignored-patterns: /**/hello/**
  routes:
    local:
      path: /local/**
      url: forward:/local
```

此时访问http://localhost:2006/myapi/local结果如下：  

![](https://mmbiz.qpic.cn/mmbiz_png/GvtDGKK4uYnG6clUSNI5WQNpdKcMPcic2zCqHLh1EPJam6mMTibP8h4hSBOic9uib7Rm7SGb4LjX1PMLPM5Fut7RvQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

我们在使用Nginx的时候，会涉及到一个请求头信息的配置，防止页面重定向后跳转到上游服务器上去，这个问题在Zuul中一样存在，假设我的feign-consumer中提供了一个接口/hello4，当访问/hello4接口的时候，页面重定向到/hello，默认情况下，重定向的地址是具体的服务实例的地址，而不是API网关的跳转地址，这种做法会暴露真实的服务地址，所以需要在Zuul中配置，配置方式很简单，如下：

```text
zuul:
  add-host-header: true
```

表示API网关在进行请求路由转发前为请求设置Host头信息。

默认情况下，敏感的头信息无法经过API网关进行传递，我们可以通过如下配置使之可以传递：

```text
zuul:
  routes:
    feign-consumer:
      sensitiveHeaders:
```

在Zuul中，Ribbon和Hystrix的配置还是和之前的配置方式一致，这里我就不赘述了，如果我们想关闭Hystrix重试机制，可以通过如下方式：

关闭全局重试机制：

```text
zuul:
  retryable: false
```

关闭某一个服务的重试机制：

```text
zuul:
  routes:
    feign-consumer:
      retryable: false
```

关于Zuul中路由的配置细节我们就说到这里，有问题欢迎留言讨论。  


