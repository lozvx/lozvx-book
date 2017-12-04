面向对象编程OOP关注的是类，面向方面编程AOP关注的是方面，Aspect。

AOP术语：

* _切面（Aspect）_：一个关注点的模块化，这个关注点可能会横切多个对象。事务管理是J2EE应用中一个关于横切关注点的很好的例子。在Spring AOP中，切面可以使用[基于模式](http://shouce.jb51.net/spring/aop.html#aop-schema)）或者基于[@Aspect注解](http://shouce.jb51.net/spring/aop.html#aop-ataspectj)的方式来实现。

* _连接点（Joinpoint）_：在程序执行过程中某个特定的点，比如某方法调用的时候或者处理异常的时候。在Spring AOP中，一个连接点_总是_表示一个方法的执行。

* _通知（Advice）_：在切面的某个特定的连接点上执行的动作。其中包括了“around”、“before”和“after”等不同类型的通知（通知的类型将在后面部分进行讨论）。许多AOP框架（包括Spring）都是以_拦截器_做通知模型，并维护一个以连接点为中心的拦截器链。

* _切入点（Pointcut）_：匹配连接点的断言。通知和一个切入点表达式关联，并在满足这个切入点的连接点上运行（例如，当执行某个特定名称的方法时）。切入点表达式如何和连接点匹配是AOP的核心：Spring缺省使用AspectJ切入点语法。

* _引入（Introduction）_：用来给一个类型声明额外的方法或属性（也被称为连接类型声明（inter-type declaration））。Spring允许引入新的接口（以及一个对应的实现）到任何被代理的对象。例如，你可以使用引入来使一个bean实现IsModified接口，以便简化缓存机制。

* _目标对象（Target Object）_： 被一个或者多个切面所通知的对象。也被称做_被通知（advised）_对象。 既然Spring AOP是通过运行时代理实现的，这个对象永远是一个_被代理（proxied）_对象。

* _AOP代理（AOP Proxy）_：AOP框架创建的对象，用来实现切面契约（例如通知方法执行等等）。在Spring中，AOP代理可以是JDK动态代理或者CGLIB代理。

* _织入（Weaving）_：把切面连接到其它的应用程序类型或者对象上，并创建一个被通知的对象。这些可以在编译时（例如使用AspectJ编译器），类加载时和运行时完成。Spring和其他纯Java AOP框架一样，在运行时完成织入。

通知类型：

* _前置通知（Before advice）_：在某连接点之前执行的通知，但这个通知不能阻止连接点之前的执行流程（除非它抛出一个异常）。

* _后置通知（After returning advice）_：在某连接点正常完成后执行的通知：例如，一个方法没有抛出任何异常，正常返回。

* _异常通知（After throwing advice）_：在方法抛出异常退出时执行的通知。

* _最终通知（After \(finally\) advice）_：当某连接点退出的时候执行的通知（不论是正常返回还是异常退出）。

* _环绕通知（Around Advice）_：包围一个连接点的通知，如方法调用。这是最强大的一种通知类型。环绕通知可以在方法调用前后完成自定义的行为。它也会选择是否继续执行连接点或直接返回它自己的返回值或抛出异常来结束执行。

Spring AOP：

Spring AOP 默认使用JDK动态代理作为AOP代理。所以只能对接口做代理。

Spring AOP 也能用CGLIB代理，在一个类没有实现一个接口时，会默认用CGLIB代理。也可以强制使用CGLIB

强制使用CGLIB

```
<aop:config proxy-target-class="true">
    <!-- other beans defined here... -->
</aop:config>
```

AspectJ

```
<aop:aspectj-autoproxy proxy-target-class="true"/>
```



