# Eureka

**Netflix Eureka**

服务中心，云端服务发现，一个基于 REST 的服务，用于定位服务，以实现云端中间层服务发现和故障转移。这个可是springcloud最牛鼻的小弟，服务中心，任何小弟需要其它小弟支持什么都需要从这里来拿，同样的你有什么独门武功的都赶紧过报道，方便以后其它小弟来调用；它的好处是你不需要直接找各种什么小弟支持，只需要到服务中心来领取，也不需要知道提供支持的其它小弟在哪里，还是几个小弟来支持的，反正拿来用就行，服务中心来保证稳定性和质量。



Eureka的配置大多都在下面两个Bean中

EurekaClientConfigBean

EurekaInstanceConfigBean





#### 健康检查

```javascript
# 健康检查eureka:  client:      healthcheck:          enabled: true
```

Eureka的健康检查可以通过client配置开启，可以通过引入actuator提供。如果不开启健康检查，在client端意外停止时，eureka server是不会感知到的，还是UP状态

可以通过直接kill进程模拟，正常关闭应用是可以往server端发送。



#### EurekaClient

可以通过EurekaClient获取注册在eureka server上的instance。

```text

InstanceInfo instance = discoveryClient.getNextServerFromEureka("EUREKA-PRODUCER", false);
```



也可以使用DiscoveryClient

```text

@Autowiredprivate DiscoveryClient discoveryClient;public String serviceUrl() {    List<ServiceInstance> list = discoveryClient.getInstances("STORES");    if (list != null && list.size() > 0 ) {        return list.get(0).getUri();    }    return null;}
```



#### 部署

单点部署：

```text

server:  port: 8761  instance:  hostname: localhostclient:  registerWithEureka: false  fetchRegistry: false  serviceUrl:      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

多点部署：

```javascript

--- spring:  profiles: peer1eureka:  instance:    hostname: peer1  client:    serviceUrl:      defaultZone: http://peer2/eureka/--- spring:  profiles: peer2eureka:  instance:    hostname: peer2  client:    serviceUrl:      defaultZone: http://peer1/eureka/
```

