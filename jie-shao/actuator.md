# Actuator

actuator提供了健康检查，监控等服务。

只要在pom中添加starter依赖即可。

```text

<!--https://docs.spring.io/spring-boot/docs/2.0.2.RELEASE/reference/htmlsingle/#production-ready --><!-- actuator 提供健康检查,监控等--><dependency>    <groupId>org.springframework.boot</groupId>    <artifactId>spring-boot-starter-actuator</artifactId></dependency>
```

访问[http://localhost:8082/actuator](http://localhost:8082/actuator)

```javascript
{
  "_links": {
    "self": {
      "href": "http://localhost:8082/actuator",
      "templated": false
    },
    "health": {
      "href": "http://localhost:8082/actuator/health",
      "templated": false
    },
    "info": {
      "href": "http://localhost:8082/actuator/info",
      "templated": false
    }
  }
}
```

