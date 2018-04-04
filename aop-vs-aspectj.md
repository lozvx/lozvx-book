[http://www.baeldung.com/spring-aop-vs-aspectj](http://www.baeldung.com/spring-aop-vs-aspectj)

| Joinpoint | Spring AOP Supported | AspectJ Supported |
| :--- | :--- | :--- |
| Method Call | No | Yes |
| Method Execution | Yes | Yes |
| Constructor Call | No | Yes |
| Constructor Execution | No | Yes |
| Static initializer execution | No | Yes |
| Object initialization | No | Yes |
| Field reference | No | Yes |
| Field assignment | No | Yes |
| Handler execution | No | Yes |
| Advice execution | No | Yes |

1.Spring AOP 不支持方法调用，即同一类中的方法调用。

2.Spring AOP基于代理模式，需要基于委托类生成子类。所以不支持final类或者final方法

3.同时不支持构造器调用，static方法等。

4.Spring AOP是运行时织入，AspectJ是编译期织入。AspectJ需要引入AspectJ编译器，AJC。如果使用AspectJ，需要重新贬义所有包。

