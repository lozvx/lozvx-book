# Hystrix

**Netflix Hystrix**

熔断器，容错管理工具，旨在通过熔断机制控制服务和第三方库的节点,从而对延迟和故障提供更强大的容错能力。比如突然某个小弟生病了，但是你还需要它的支持，然后调用之后它半天没有响应，你却不知道，一直在等等这个响应；有可能别的小弟也正在调用你的武功绝技，那么当请求多之后，就会发生严重的阻塞影响老大的整体计划。这个时候Hystrix就派上用场了，当Hystrix发现某个小弟不在状态不稳定立马马上让它下线，让其它小弟来顶上来，或者给你说不用等了这个小弟今天肯定不行，该干嘛赶紧干嘛去别在这排队了。

Hystrix实现了CircuitBreaker机制。

### 熔断器

[https://martinfowler.com/bliki/CircuitBreaker.html](https://martinfowler.com/bliki/CircuitBreaker.html)

[https://www.sczyh30.com/posts/Microservice/circuit-breaker-pattern/](https://www.sczyh30.com/posts/Microservice/circuit-breaker-pattern/)



断路器的基本思路很简单。通过将待保护的函数调用包裹在断路器对象中，让断路器对象来监控失败。当失败次数达到特定的阈值时，断路器打开，后续对此断路器对象的访问将直接返回 error，根本不会调用受保护的函数。通常，你会想在断路器打开的时候得到某种监控预警。  


![](../../.gitbook/assets/image%20%2811%29.png)





* 关闭\(**Closed**\)：默认情况下Circuit Breaker是关闭的，此时允许操作执行。Circuit Breaker内部记录着最近失败的次数，如果对应的操作执行失败，次数就会续一次。如果在某个时间段内，失败次数（或者失败比率）达到阈值，Circuit Breaker会转换到开启\(**Open**\)状态。在开启状态中，Circuit Breaker会启用一个超时计时器，设这个计时器的目的是给集群相应的时间来恢复故障。当计时器时间到的时候，Circuit Breaker会转换到半开启\(**Half-Open**\)状态。
* 开启\(**Open**\)：在此状态下，执行对应的操作将会立即失败并且立即抛出异常。
* 半开启\(**Half-Open**\)：在此状态下，Circuit Breaker会允许执行一定数量的操作。如果所有操作全部成功，Circuit Breaker就会假定故障已经恢复，它就会转换到关闭状态，并且重置失败次数。如果其中 **任意一次** 操作失败了，Circuit Breaker就会认为故障仍然存在，所以它会转换到开启状态并再次开启计时器（再给系统一些时间使其从失败中恢复）。

![](../../.gitbook/assets/image%20%2820%29.png)



### 示例

```text
​
@SpringBootApplication@EnableCircuitBreakerpublic class Application {   public static void main(String[] args) {      new SpringApplicationBuilder(Application.class).web(true).run(args);   }}@Componentpublic class StoreIntegration {   @HystrixCommand(fallbackMethod = "defaultStores")   public Object getStores(Map<String, Object> parameters) {      //do stuff that might fail   }   public Object defaultStores(Map<String, Object> parameters) {      return /* something useful */;   }}
```

也可以用Feign

```text

@FeignClient(name= "eureka-producer", fallback = HelloRemoteHystrix.class)public interface HelloRemote {   @RequestMapping(value = "/hello")   String hello(@RequestParam(value = "name") String name);}
```



### Hystrix Dashboard

1. 引入依赖

```text
<!-- 包含进hystrix的dashboard web http://localhost:8082/hystrix -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
</dependency>
```

 2. 开启

```text
@SpringBootApplication
@EnableDiscoveryClient
@EnableFeignClients(basePackages = "io.lozvx")
@EnableHystrix
@EnableHystrixDashboard
@ComponentScan(basePackages = "io.lozvx")
public class Application {
    
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

 3.  引入actuator并开启所有

```text
#actuator中开启所有 包括hystrix.stream http://localhost:8082/actuator/hystrix.stream
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

 4. 访问地址 [http://localhost:8082/hystrix](http://localhost:8082/hystrix)

![](../../.gitbook/assets/image%20%2821%29.png)

![](../../.gitbook/assets/image%20%2814%29.png)



### Turbine（涡轮）

> Looking at an individual instance’s Hystrix data is not very useful in terms of the overall health of the system. Turbine is an application that aggregates all of the relevant /hystrix.stream endpoints into a combined /turbine.stream for use in the Hystrix Dashboard. Individual instances are located through Eureka.

Turbine是hystrix.stream集群的整合。

参考：[https://blog.csdn.net/u012702547/article/details/78224483](https://blog.csdn.net/u012702547/article/details/78224483)

