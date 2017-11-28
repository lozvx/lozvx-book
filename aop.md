### 什么是面向切面（AOP）？

#### 概念

* 在软件业，AOP为Aspect Oriented Programming的缩写，意为：面向切面编程
* AOP是一种编程范式，隶属于软工范畴，指导开发者如何组织程序结构
* AOP最早由AOP联盟的组织提出的,制定了一套规范.Spring将AOP思想引入到框架中,必须遵守AOP联盟的规范
* 通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术
* AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型
* 利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率

> 其实AOP可以在不修改源代码的前提下，对程序进行增强！！

#### Spring框架的AOP的底层实现

1. Spring框架的AOP技术底层也是采用的代理技术，代理的方式提供了两种

> 1. 基于JDK的动态代理

```
 必须是面向接口的，只有实现了具体接口的类才能生成代理对象

```

> 1. 基于CGLIB动态代理

```
对于没有实现了接口的类，也可以产生代理，产生这个类的子类的方式

```

1. Spring的传统AOP中根据类是否实现接口，来采用不同的代理方式

> 1. 如果实现类接口，使用JDK动态代理完成AOP
> 2. 如果没有实现接口，采用CGLIB动态代理完成AOP

#### JDK的动态代理

**注意：需要实现类接口**

> 例子：假设我有两个工作，工作1，工作2.

```
//写一个接口
public
interface
Working
{
    
void
wokingOne
()
;

    
void
WorkingTwo
()
;
}

```

```
//接口实现类
public
class
WorkingImpl
implements
Working
 {
    
@Override

    public void wokingOne() {
        System
.out
.println
(
"做任务1"
);
    }

    
@Override

    public void WorkingTwo() {
        System
.out
.println
(
"做任务2"
);
    }
}

```

> 好的，现在我要先做任务1，然后再做任务2我们的写法为:

```
public
static
void
main
(String[] args)
{
        Working working = 
new
 WorkingImpl();
        working.wokingOne();
//做任务1

        working.WorkingTwo();
//做任务2

    }
 

```

> 好的精彩的地方来了,我再做任务2之前我要先休息10分钟，但是不能修改源代码。怎么办呢？这时候就用到我们的JDK动态代理了。代码如下：

> 先写一个代理的工具类。再做任务2前我们休息十分钟

```
public
class
MyProxyUtils
{
    
public
static
 Working 
getProxy
(
final
 Working working)
{
        
// 使用Proxy类生成代理对象

        Working proxy = (Working) Proxy.newProxyInstance(working.getClass().getClassLoader(),
                working.getClass().getInterfaces(), 
new
 InvocationHandler() {

                    
// 代理对象方法一直线，invoke方法就会执行一次
public
 Object 
invoke
(Object proxy, Method method, Object[] args)
throws
 Throwable 
{
                        
//再做工作2之前我先休息10分钟
if
 (
"WorkingTwo"
.equals(method.getName())) {
                            System.out.println(
"休息10分钟"
);
                        }
                        
//工作继续进行下去
return
 method.invoke(working, args);
                    }
                });
        
// 返回代理对象
return
 proxy;
    }
}

```

```
public
static
void
main
(String[] args)
{
        Working working = 
new
 WorkingImpl();
        Working proxy = MyProxyUtils.getProxy(working);
        proxy.wokingOne();
        proxy.WorkingTwo();

    }

```

运行的结果可想而知：

```
做任务1
休息10分钟
做任务2

```

#### CGLIB动态代理

**注意：没有实现类接口**

> CGLIB也是一个java项目，所以要使用它就要引入CGLIB的开发的jar包，**因为在Spring框架核心包（core）中已经引入了CGLIB的开发包了**。所以直接引入Spring核心开发包即可

**好我们同样使用上面的例子来做事例吧**

```
//工作类
public
class
Working
{

    
public
void
wokingOne
()
{
        System.out.println(
"做任务1"
);
    }

    
public
void
WorkingTwo
()
{
        System.out.println(
"做任务2"
);
    }
}

```

> new一个对象调用任务1 任务2得到的结果这个就没必要说了。我们重点看下怎么使用CGLIB来在不改变源码的情况下，做任务之前休息十分钟。

```
public
static
 Working 
getProxy
()
{
        
// 创建CGLIB核心的类

        Enhancer enhancer = 
new
 Enhancer();
        
// 设置父类

        enhancer.setSuperclass(Working.class);
        
// 设置回调函数

        enhancer.setCallback(
new
 MethodInterceptor() {
            
@Override
public
 Object 
intercept
(Object obj, Method method, Object[] args,
                    MethodProxy methodProxy)
throws
 Throwable 
{
                
if
(
"WorkingTwo"
.equals(method.getName())){
                    
// 休息10分钟

                    System.out.println(
"休息10分钟..."
);
                }
                
return
 methodProxy.invokeSuper(obj, args);
            }
        });
        
// 生成代理对象

        Working proxy = (Working) enhancer.create();
        
return
 proxy;
    }

```

```
public
static
void
main
(String[] args)
{
        Working working = 
new
 WorkingImpl();
        Working proxy = MyCglibUtils.getProxy(working);
        proxy.wokingOne();
        proxy.WorkingTwo();

    }

```

最后的运行结果也是相同的：

```
做任务1
休息10分钟
做任务2

```

#### 分析

我上面简单分析了一下spring AOP的底层实现的两种方式：JDK动态代理，CGLIB。在Spring框架中它会自动选择使用哪一种方式。如果有接口实现类，那就使用jdk动态代理，没有接口就使用CGLIB。有了这个功能那么我们修改东西就方便多了。假设我再做任务一前我需要记录下日志。我就直接写一个切面类，直接去记录日志。都不用修改本身的源码。

