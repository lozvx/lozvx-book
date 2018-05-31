# Eureka

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

@Autowiredprivate DiscoveryClient discoveryClient;
public String serviceUrl() {    List<ServiceInstance> list = discoveryClient.getInstances("STORES");    if (list != null && list.size() > 0 ) {
        return list.get(0).getUri();    }
    return null;}
```

