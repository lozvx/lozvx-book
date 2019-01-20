# Eureka Startup



{% embed url="https://spring.io/guides/gs/service-registration-and-discovery/" %}



{% embed url="https://github.com/Netflix/eureka/wiki/Eureka-at-a-glance" %}





![](../../.gitbook/assets/image%20%2822%29.png)



Eureka分为Client和Server两部分。Client发送请求到Server，Server接受Client的请求。

### Client

1. Maven 依赖

```text
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
		</dependency>
```

2 开启

```text
@EnableEurekaServer
@SpringBootApplication
public class EurekaServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(EurekaServiceApplication.class, args);
    }
}
```

3 配置服务器 端口等

```text
spring:
  application:
    name: eureka-consumer
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
# 健康检查
    healthcheck:
      enabled: true
```

### Server

1 Maven依赖

```text
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

2开启

```text
@EnableEurekaServer
@SpringBootApplication
public class EurekaServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(EurekaServiceApplication.class, args);
    }
}

```

3 配置

```text
server:
  port: 8761

eureka:
  client:
    registerWithEureka: false
    fetchRegistry: false
  server:
    waitTimeInMsWhenSyncEmpty: 0
```

