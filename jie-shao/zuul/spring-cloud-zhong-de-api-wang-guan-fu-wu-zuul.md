# Spring Cloud中的API网关服务Zuul

{% embed data="{\"url\":\"https://mp.weixin.qq.com/s/BWhh2HD0VVqzypxkBFU7eg\",\"type\":\"link\",\"icon\":{\"type\":\"icon\",\"url\":\"https://res.wx.qq.com/mmbizwap/zh\_CN/htmledition/images/icon/common/favicon22c41b.ico\",\"aspectRatio\":0}}" %}



到目前为止，我们Spring Cloud中的内容已经介绍了很多了，Ribbon、Hystrix、Feign这些知识点大家都耳熟能详了，我们在前文也提到过微服务就是把一个大的项目拆分成很多小的独立模块，然后通过服务治理让这些独立的模块配合工作等。那么大家来想这样两个问题：1.如果我的微服务中有很多个独立服务都要对外提供服务，那么对于开发人员或者运维人员来说，他要如何去管理这些接口?特别是当项目非常大非常庞杂的情况下要如何管理?2.权限管理也是一个老生常谈的问题，在微服务中，一个独立的系统被拆分成很多个独立的模块，为了确保安全，我难道需要在每一个模块上都添加上相同的鉴权代码来确保系统不被非法访问?如果是这样的话，那么工作量就太大了，而且维护也非常不方便。

为了解决上面提到的问题，我们引入了API网关的概念，API网关是一个更为智能的应用服务器，它有点类似于我们微服务架构系统的门面，所有的外部访问都要先经过API网关，然后API网关来实现请求路由、负载均衡、权限验证等功能。Spring Cloud中提供的Spring Cloud Zuul实现了API网关的功能，本文我们就先来看看Spring Cloud Zuul的一个基本使用。

本文是Spring Cloud系列的第十九篇文章，了解前十八篇文章内容有助于更好的理解本文：

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

### 构建网关

网关的构建我们通过下面三个步骤来实现。

#### 1.创建Spring Boot工程并添加依赖

首先我们创建一个普通的Spring Boot工程名为api-gateway，然后添加相关依赖，这里我们主要添加两个依赖spring-cloud-starter-zuul和spring-cloud-starter-eureka，spring-cloud-starter-zuul依赖中则包含了ribbon、hystrix、actuator等，如下：

```text
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.7.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <java.version>1.8</java.version>
    <spring-cloud.version>Dalston.SR3</spring-cloud.version>
</properties>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-zuul</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-eureka</artifactId>
    </dependency>
</dependencies>
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

#### 2.添加注解

然后在入口类上添加@EnableZuulProxy注解表示开启Zuul的API网关服务功能，如下：

```text
@SpringBootApplication
@EnableZuulProxy
public class ApiGatewayApplication {

    public static void main(String[] args) {
        SpringApplication.run(ApiGatewayApplication.class, args);
    }
}
```

#### 3.配置路由规则

application.properties文件中的配置可以分为两部分，一部分是Zuul应用的基础信息，还有一部分则是路由规则，如下：

```text
# 基础信息配置
spring.application.name=api-gateway
server.port=2006
# 路由规则配置
zuul.routes.api-a.path=/api-a/**
zuul.routes.api-a.serviceId=feign-consumer

# API网关也将作为一个服务注册到eureka-server上
eureka.client.service-url.defaultZone=http://localhost:1111/eureka/
```

我们在这里配置了路由规则所有符合/api-a/\*\*的请求都将被转发到feign-consumer服务上，至于feign-consumer服务的地址到底是什么则由eureka-server去分析，我们这里只需要写上服务名即可。以上面的配置为例，如果我请求http://localhost:2006/api-a/hello1接口则相当于请求http://localhost:2005/hello1\(我这里feign-consumer的地址为http://localhost:2005\)，我们在路由规则中配置的api-a是路由的名字，可以任意定义，但是一组path和serviceId映射关系的路由名要相同。

OK，做好这些之后，我们依次启动我们的eureka-server、provider和feign-consumer，然后访问如下地址http://localhost:2006/api-a/hello1，访问结果如下：

![](https://mmbiz.qpic.cn/mmbiz_png/GvtDGKK4uYnG6clUSNI5WQNpdKcMPcic2yDOpO43F8TmOKT7icicGc0G26VpPVWj05tMDTtpSWsWYykzdtiaJ4t58Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

看到这个效果说明我们的API网关服务已经构建成功了，我们发送的符合路由规则的请求自动被转发到相应的服务上去处理了。

### 请求过滤

构建好了网关，接下来我们就来看看如何利用网关来实现一个简单的权限验证。这里就涉及到了Spring Cloud Zuul中的另外一个核心功能：请求过滤。请求过滤有点类似于Java中Filter过滤器，先将所有的请求拦截下来，然后根据现场情况做出不同的处理，这里我们就来看看Zuul中的过滤器要如何使用。很简单，两个步骤：

#### 1.定义过滤器

首先我们定义一个过滤器继承自ZuulFilter，如下：

```text
public class PermisFilter extends ZuulFilter {
    @Override
    public String filterType() {
        return "pre";
    }

    @Override
    public int filterOrder() {
        return 0;
    }

    @Override
    public boolean shouldFilter() {
        return true;
    }

    @Override
    public Object run() {
        RequestContext ctx = RequestContext.getCurrentContext();
        HttpServletRequest request = ctx.getRequest();
        String login = request.getParameter("login");
        if (login == null) {
            ctx.setSendZuulResponse(false);
            ctx.setResponseStatusCode(401);
            ctx.addZuulResponseHeader("content-type","text/html;charset=utf-8");
            ctx.setResponseBody("非法访问");
        }
        return null;
    }
}
```

关于这个类我说如下几点：

> 1.filterType方法的返回值为过滤器的类型，过滤器的类型决定了过滤器在哪个生命周期执行，pre表示在路由之前执行过滤器，其他可选值还有post、error、route和static，当然也可以自定义。  
> 2.filterOrder方法表示过滤器的执行顺序，当过滤器很多时，这个方法会有意义。  
> 3.shouldFilter方法用来判断过滤器是否执行，true表示执行，false表示不执行，在实际开发中，我们可以根据当前请求地址来决定要不要对该地址进行过滤，这里我直接返回true。  
> 4.run方法则表示过滤的具体逻辑，假设请求地址中携带了login参数的话，则认为是合法请求，否则就是非法请求，如果是非法请求的话，首先设置ctx.setSendZuulResponse\(false\);表示不对该请求进行路由，然后设置响应码和响应值。这个run方法的返回值在当前版本\(Dalston.SR3\)中暂时没有任何意义，可以返回任意值。

#### 2.配置过滤器Bean

然后在入口类中配置相关的Bean即可，如下：

```text
@Bean
PermisFilter permisFilter() {
    return new PermisFilter();
}
```

此时，如果我们访问http://localhost:2006/api-a/hello1，结果如下：  

![](https://mmbiz.qpic.cn/mmbiz_png/GvtDGKK4uYnG6clUSNI5WQNpdKcMPcic2DL9DSNaGLxhpq4DTqXqvTTIAZkg6cuK7WeVpLRepj7AvK2Lia9Nh2Aw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

如果给请求地址加上login参数，则结果如下：

![](https://mmbiz.qpic.cn/mmbiz_png/GvtDGKK4uYnG6clUSNI5WQNpdKcMPcic22qlZYZe9P9LCcjIw9ibYicfBIPJezcb7tg1SGPyhkuKjQicgamoRb7u9A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

### 总结

到这里小伙伴们应该已经见识到Spring Cloud Zuul的强大之处了吧，API网关作为系统的的统一入口，将微服务中的内部细节都屏蔽掉了，而且能够自动的维护服务实例，实现负载均衡的路由转发，同时，它提供的过滤器为所有的微服务提供统一的权限校验机制，使得服务自身只需要关注业务逻辑即可。

