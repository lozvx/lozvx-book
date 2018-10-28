# Spring Cloud中声明式服务调用Feign

{% embed url="https://mp.weixin.qq.com/s/6ApgudWoUzAhg6BEcRmSBw" %}



上篇文章我们了解了Feign的基本使用，在HelloService类中声明接口时，我们发现这里的代码可以直接从服务提供者的Controller中复制过来，这些可以复制的代码Spring Cloud Feign对它进行了进一步的抽象，这里就用到了Feign的继承特性，本文我们就来看看如何利用Feign的继承特性来进一步简化我们的代码。  


### 创建公共接口

首先我们来创建一个普通的maven工程，叫做hello-service-api，由于我们要在这一个项目中使用SpringMVC的注解，因此创建成功之后，需要添加spring-boot-starter-web依赖，如下：

```text
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.sang</groupId>
    <artifactId>commons</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

commons是我的另外一个maven项目，里边提供了Book这个类，因为我的hello-service-api一会要用到Book类，当然这里你也可以直接在hello-service-api中定义一个Book类。

项目创建好之后，我们在这个项目中提供一个HelloSerivce接口，如下：

```text
@RequestMapping("/hs2")
public interface HelloService {
    @RequestMapping(value = "/hello1", method = RequestMethod.GET)
    String hello(@RequestParam("name") String name);

    @RequestMapping(value = "/hello2", method = RequestMethod.GET)
    Book hello(@RequestHeader("name") String name, @RequestHeader("author") String author, @RequestHeader("price") Integer price) throws UnsupportedEncodingException;

    @RequestMapping(value = "/hello3", method = RequestMethod.POST)
    String hello(@RequestBody Book book);
}
```

小伙伴们有木有觉得眼熟呢?就是我们上文在HelloService中定义的内容。为了和上文的HelloService进行区分，这次我做了请求窄化，给请求定义了前缀/hs2。

### 服务提供者中实现接口

hello-service-api工程写好之后，我们在服务提供者中添加对hello-service-api工程的依赖，如下：

```text
<dependency>
    <groupId>org.sang</groupId>
    <artifactId>hello-service-api</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>
```

然后在服务提供者中创建BookController2实现HelloService接口，如下：

```text
@RestController
public class BookController2 implements HelloService {
    @Override
    public String hello(@RequestParam("name") String name) {
        return "hello " + name + "!";
    }

    @Override
    public Book hello(@RequestHeader("name") String name, @RequestHeader("author") String author, @RequestHeader("price") Integer price) throws UnsupportedEncodingException {
        org.sang.Book book = new org.sang.Book();
        book.setName(URLDecoder.decode(name,"UTF-8"));
        book.setAuthor(URLDecoder.decode(author,"UTF-8"));
        book.setPrice(price);
        System.out.println(book);
        return book;
    }

    @Override
    public String hello(@RequestBody Book book) {
        return "书名为：" + book.getName() + ";作者为：" + book.getAuthor();
    }
}
```

实现了HelloService接口当然就要实现HelloService接口中的方法，方法的实现还是和上文的一样。不同的是我这里不需要在方法上面添加@RequestMapping注解，这些注解在父接口中都有，不过在Controller上还是要添加@RestController注解，**另外需要注意的是，方法中的参数@RequestHeader和@RequestBody注解还是要添加，@RequestParam注解可以不添加。**  

### 服务消费者中继承接口

写完了服务提供者之后，接下来我们再来看看服务消费者。首先在服务消费者中添加对hello-service-api的依赖，然后新建一个HelloService2类继承hello-service-api中的HelloService接口，如下：

```text
@FeignClient("hello-service")
public interface HelloService2 extends HelloService {
}
```

这个接口中不需要添加任何方法，方法都在父接口中，这里只需要在类上面添加@FeignClient\(“hello-service”\)注解来绑定服务即可。

然后在Controller中提供相应的测试接口，如下：

```text
@RestController
public class FeignConsumerController {

    @Autowired
    HelloService2 helloService2;

    @RequestMapping("/hello1")
    public String hello1() {
        return helloService2.hello("张三");
    }

    @RequestMapping(value = "/hello2")
    public Book hello2() throws UnsupportedEncodingException {
        Book book = helloService2.hello(URLEncoder.encode("三国演义","UTF-8"), URLEncoder.encode("罗贯中","UTF-8"), 33);
        System.out.println(book);
        return book;
    }

    @RequestMapping("/hello3")
    public String hello3() {
        Book book = new Book();
        book.setName("红楼梦");
        book.setPrice(44);
        book.setAuthor("曹雪芹");
        return helloService2.hello(book);
    }
}
```

### 测试

最后，我们依次启动eureka-server、provider和feign-consumer，然后访问相应的接口，测试结果如下：

http://localhost:2005/hello1：  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

http://localhost:2005/hello2：  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

http://localhost:2005/hello3：  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 总结

OK，通过上面的介绍小伙伴们应该已经体会到了Feign继承特性的方便之处了，这种方式用起来确实很方面，但是也带来一个问题，就是服务提供者和服务消费者的耦合度太高，此时如果服务提供者修改了一个接口的定义，服务消费者可能也得跟着变化，进而带来很多未知的工作量，因此小伙伴们在使用继承特性的时候，要慎重考虑。

关于Spring Cloud中Feign继承特性我们就介绍到这里，有问题欢迎留言讨论



