参考：[/www.jianshu.com/p/6f6bb2f0ece9](/www.jianshu.com/p/6f6bb2f0ece9)

* 静态代理：代理类是在编译时就实现好的。也就是说 Java 编译完成后代理类是一个实际的 class 文件。

```java
public class ProxyDemo {
    public static void main(String args[]){
        RealSubject subject = new RealSubject();
        Proxy p = new Proxy(subject);
        p.request();
    }
}

interface Subject{
    void request();
}

class RealSubject implements Subject{
    public void request(){
        System.out.println("request");
    }
}

class Proxy implements Subject{
    private Subject subject;
    public Proxy(Subject subject){
        this.subject = subject;
    }
    public void request(){
        System.out.println("PreProcess");
        subject.request();
        System.out.println("PostProcess");
    }
}
```

* 动态代理：代理类是在运行时生成的也就是说 Java 编译完之后并没有实际的 class 文件，而是在运行时动态生成的类字节码，并加载到JVM中。

首先先说明几个词：

```
委托类和委托对象：委托类是一个类，委托对象是委托类的实例。

代理类和代理对象：代理类是一个类，代理对象是代理类的实例。
```

Java实现动态代理的大致步骤如下：

```
定义一个委托类和公共接口。

自己定义一个类（调用处理器类，即实现 InvocationHandler 接口），这个类的目的是指定运行时将生成的代理类需要完成的具体任务（包括Preprocess和Postprocess），即代理类调用任何方法都会经过这个调用处理器类（在本文最后一节对此进行解释）。

生成代理对象（当然也会生成代理类），需要为他指定\(1\)委托对象\(2\)实现的一系列接口\(3\)调用处理器类的实例。因此可以看出一个代理对象对应一个委托对象，对应一个调用处理器实例。
```

Java 实现动态代理主要涉及以下几个类：

```
java.lang.reflect.Proxy: 这是生成代理类的主类，通过 Proxy 类生成的代理类都继承了 Proxy 类，即 DynamicProxyClass extends Proxy。

java.lang.reflect.InvocationHandler: 这里称他为"调用处理器"，他是一个接口，我们动态生成的代理类需要完成的具体内容需要自己定义一个类，而这个类必须实现 InvocationHandler 接口。
```

```java
static Object newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler handler)
{
    //1. 根据类加载器和接口创建代理类
    Class clazz = Proxy.getProxyClass(loader, interfaces); 
    //2. 获得代理类的带参数的构造函数
    Constructor constructor = clazz.getConstructor(new Class[] { InvocationHandler.class });
    //3. 创建代理对象，并制定调用处理器实例为参数传入
    Interface Proxy = (Interface)constructor.newInstance(new Object[] {handler});
}
```

Demo:

```java

package reflect;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyTest {

    public static void main(String[] args) {
        RealSubject realSubject = new RealSubject();    //1.创建委托对象
        ProxyHandler handler = new ProxyHandler(realSubject);    //2.创建调用处理器对象
        Subject proxySubject = (Subject) Proxy.newProxyInstance(RealSubject.class.getClassLoader(),
                RealSubject.class.getInterfaces(), handler);    //3.动态生成代理对象
        proxySubject.request();    //4.通过代理对象调用方法
    }

    /**
     * 接口
     */
    interface Subject {
        void request();
    }

    /**
     * 委托类
     */
    static class RealSubject implements Subject {
        public void request() {
            System.out.println("====RealSubject Request====");
        }
    }

    /**
     * 代理类的调用处理器
     */
    static class ProxyHandler implements InvocationHandler {
        private Subject subject;

        public ProxyHandler(Subject subject) {
            this.subject = subject;
        }

        public Object invoke(Object proxy, Method method, Object[] args)
                throws Throwable {
            System.out.println("====before====");//定义预处理的工作，当然你也可以根据 method 的不同进行不同的预处理工作
            Object result = method.invoke(subject, args);
            System.out.println("====after====");
            return result;
        }
    }
}
```



