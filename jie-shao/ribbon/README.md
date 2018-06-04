# Ribbon

**Ribbon**   
是一个基于 HTTP 和 TCP 客户端的负载均衡器   
它可以在客户端配置 ribbonServerList（服务端列表），然后轮询请求以实现均衡负载。

**Feign**   
Spring Cloud Netflix 的微服务都是以 HTTP 接口的形式暴露的，所以可以用 Apache 的 HttpClient 或 Spring 的 RestTemplate 去调用，而 Feign 是一个使用起来更加方便的 HTTP 客戶端，使用起来就像是调用自身工程的方法，而感觉不到是调用远程方法。

注意： 在Feign里面已经包含了 spring-cloud-starter-ribbon，即Feign中也使用Ribbon

参考：[https://mp.weixin.qq.com/s/uvJDmN2f9y3EEI6A3ss\_aQ](https://mp.weixin.qq.com/s/uvJDmN2f9y3EEI6A3ss_aQ)

Ribbon实现的是Spring-cloud-commons中的loadbalancer接口

![](../../.gitbook/assets/image%20%282%29.png)

