https://www.eclipse.org/aspectj/doc/released/progguide/index.html

https://www.eclipse.org/aspectj/doc/released/adk15notebook/index.html



Aspject：

开启@AspectJ，需要引入aspectjweaver.jar并且开启配置。

注解

```java
@Configuration
@EnableAspectJAutoProxy
public class AppConfig {

}
```

或XML

```
<aop:aspectj-autoproxy/>
```

声明一个切面

```java
package org.xyz;
import org.aspectj.lang.annotation.Aspect;

@Aspect
public class NotVeryUsefulAspect {

}
```

声明切入点

下面这个例子将匹配到所有叫transfer的方法。

```java
@Pointcut("execution(* transfer(..))")// the pointcut expression
private void anyOldTransfer() {}// the pointcut signature
```



Spring AOP 支持下面的AspectJ切入点表达式。

`这些均不支持call, get, set, preinitialization, staticinitialization, initialization, handler, adviceexecution, withincode, cflow, cflowbelow, if, @this`, and `@withincode`

支持：

* _execution_ - for matching method execution join points, this is the primary pointcut designator you will use when working with Spring AOP

* _within_ - limits matching to join points within certain types \(simply the execution of a method declared within a matching type when using Spring AOP\)

* _this_ - limits matching to join points \(the execution of methods when using Spring AOP\) where the bean reference \(Spring AOP proxy\) is an instance of the given type

* _target_ - limits matching to join points \(the execution of methods when using Spring AOP\) where the target object \(application object being proxied\) is an instance of the given type

* _args_ - limits matching to join points \(the execution of methods when using Spring AOP\) where the arguments are instances of the given types

* _@target_ - limits matching to join points \(the execution of methods when using Spring AOP\) where the class of the executing object has an annotation of the given type

* _@args_ - limits matching to join points \(the execution of methods when using Spring AOP\) where the runtime type of the actual arguments passed have annotations of the given type\(s\)

* _@within_ - limits matching to join points within types that have the given annotation \(the execution of methods declared in types with the given annotation when using Spring AOP\)

* _@annotation_ - limits matching to join points where the subject of the join point \(method being executed in Spring AOP\) has the given annotation



