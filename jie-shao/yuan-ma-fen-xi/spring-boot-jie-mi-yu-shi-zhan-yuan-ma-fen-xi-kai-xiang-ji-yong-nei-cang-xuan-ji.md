# Spring Boot 揭秘与实战 源码分析 - 开箱即用，内藏玄机

[http://blog.720ui.com/2016/springboot\_source\_autoconfigure/](http://blog.720ui.com/2016/springboot_source_autoconfigure/)

文章目录

1. [1. 开箱即用，内藏玄机](http://blog.720ui.com/2016/springboot_source_autoconfigure/#%E5%BC%80%E7%AE%B1%E5%8D%B3%E7%94%A8%EF%BC%8C%E5%86%85%E8%97%8F%E7%8E%84%E6%9C%BA)
2. [2. 总结](http://blog.720ui.com/2016/springboot_source_autoconfigure/#%E6%80%BB%E7%BB%93)
3. [3. 源代码](http://blog.720ui.com/2016/springboot_source_autoconfigure/#%E6%BA%90%E4%BB%A3%E7%A0%81)

![&#x5FAE;&#x4FE1;&#x516C;&#x4F17;&#x53F7;](http://7xivgs.com1.z0.glb.clouddn.com/%E5%85%AC%E4%BC%97%E5%8F%B7%E6%8E%A8%E9%80%8102.png)

Spring Boot提供了很多”开箱即用“的依赖模块，那么，Spring Boot 如何巧妙的做到开箱即用，自动配置的呢？

### 开箱即用，内藏玄机 {#开箱即用，内藏玄机}

Spring Boot提供了很多”开箱即用“的依赖模块，都是以spring-boot-starter-xx作为命名的。例如，之前提到的 spring-boot-starter-redis、spring-boot-starter-data-mongodb、spring-boot-starter-data-elasticsearch 等。

Spring Boot 的开箱即用是一个很棒的设计，给开发者带来很大的便利。开发者只要在 Maven 的 pom 文件中添加相关依赖后，Spring Boot 就会针对这个应用自动创建和注入需要的 Spring Bean 到上下文中。

那么，Spring Boot 如何巧妙的做到开箱即用，自动配置的呢？

现在，我们通过源码深入分析下 Spring Boot 的实现原理吧。

自动注入的核心在于 spring-boot-autoconfigure.jar 这个类库。在分析之前，我们先来看几个文件。

```text
@Configuration@ConditionalOnClass({ JedisConnection.class, RedisOperations.class, Jedis.class })@EnableConfigurationPropertiespublic class RedisAutoConfiguration {}
```

```text
@Configuration@ConditionalOnClass({ Mongo.class, MongoRepository.class })@ConditionalOnMissingBean({ MongoRepositoryFactoryBean.class,    MongoRepositoryConfigurationExtension.class })@ConditionalOnProperty(prefix = "spring.data.mongodb.repositories",     name = "enabled", havingValue = "true", matchIfMissing = true)@Import(MongoRepositoriesAutoConfigureRegistrar.class)@AutoConfigureAfter(MongoDataAutoConfiguration.class)public class MongoRepositoriesAutoConfiguration {}
```

```text
@Configuration@ConditionalOnClass({ Client.class, TransportClientFactoryBean.class,    NodeClientFactoryBean.class })@EnableConfigurationProperties(ElasticsearchProperties.class)public class ElasticsearchAutoConfiguration implements DisposableBean {}
```

上面三个源码分别对应Redis、MongoDB、ElasticSearch。通过对比，我们会发现它们都有一个特点，都存在 @ConditionalOnClass 注解。这个注解就是问题的关键所在。

@ConditionalOnClass 是什么作用呢？我们先来大概理解下面的代码。

![](http://7xivgs.com1.z0.glb.clouddn.com/springboot-conditionalonclass.jpg)

源码中的方法主要是是将 @ConditionalOnClass 的参数中对应的类进行查询和匹配。

那么，查询的目的是什么呢？查询的目的在于， @ConditionalOnClass 参数中对应的类在 classpath 目录下存在时，才会去解析对应的配置类，否则不解析该注解修饰的配置类。

因此，Spring Boot 的开箱即用的实现原理，就很好简单，用一句话就可以概括了。

Spring Boot 内部提供了很多自动化配置的类，例如，RedisAutoConfiguration 、MongoRepositoriesAutoConfiguration 、ElasticsearchAutoConfiguration ， 这些自动化配置的类会判断 classpath 中是否存在自己需要的那个类，如果存在则会自动配置相关的配置，否则就不会自动配置，因此，开发者在 Maven 的 pom 文件中添加相关依赖后，这些依赖就会下载很多 jar 包到 classpath 中，有了这些 lib 就会触发自动化配置，所以，我们就能很便捷地使用对于的模块功能了。

### 总结 {#总结}

Spring Boot 如何巧妙的做到开箱即用，自动配置的呢？实际上，Spring Boot 内部提供了很多自动化配置的类这些自动化配置的类会判断 classpath 中是否存在自己需要的那个类，如果存在则会自动配置相关的配置，否则就不会自动配置，因此，开发者在 Maven 的 pom 文件中添加相关依赖后，这些依赖就会下载很多 jar 包到 classpath 中，有了这些 lib 就会触发自动化配置。

### 源代码 {#源代码}

> 相关示例完整代码： [springboot-action](https://github.com/lianggzone/springboot-action)

