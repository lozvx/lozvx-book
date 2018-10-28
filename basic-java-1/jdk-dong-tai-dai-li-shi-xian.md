# JDK动态代理实现

{% embed url="https://my.oschina.net/robinyao/blog/811193" %}





本文主要介绍JDK动态代理的基本原理，让大家更深刻的理解JDK Proxy,知其然知其所以然。明白JDK动态代理真正的原理及其生成的过程，我们以后写JDK Proxy可以不用去查demo，就可以徒手写个完美的Proxy。下面首先来个简单的Demo,后续的分析过程都依赖这个Demo去介绍，例子采用JDK1.8运行。

\#\#JDK Proxy HelloWorld

```text
package com.yao.proxy;
/**
 * Created by robin
 */
public interface Helloworld {
    void sayHello();
}
```

```text
package com.yao.proxy;
import com.yao.HelloWorld;
/**
 * Created by robin
 */
public class HelloworldImpl implements HelloWorld {
    public void sayHello() {
        System.out.print("hello world");
    }
}
```

```text
package com.yao.proxy;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
/**
 * Created by robin
 */
public class MyInvocationHandler implements InvocationHandler{
    private Object target;
    public MyInvocationHandler(Object target) {
        this.target=target;
    }
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("method :"+ method.getName()+" is invoked!");
        return method.invoke(target,args);
    }
}
```

```text
package com.yao.proxy;
import com.yao.HelloWorld;
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Proxy;

/**
 * Created by robin
 */
public class JDKProxyTest {
    public static void main(String[]args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        //这里有两种写法，我们采用略微复杂的一种写法，这样更有助于大家理解。
        Class<?> proxyClass= Proxy.getProxyClass(JDKProxyTest.class.getClassLoader(),HelloWorld.class);
        final Constructor<?> cons = proxyClass.getConstructor(InvocationHandler.class);
        final InvocationHandler ih = new MyInvocationHandler(new HelloworldImpl());
        HelloWorld helloWorld= (HelloWorld)cons.newInstance(ih);
        helloWorld.sayHello();
        
        //下面是更简单的一种写法，本质上和上面是一样的
        /* 
        HelloWorld helloWorld=(HelloWorld)Proxy.
                 newProxyInstance(JDKProxyTest.class.getClassLoader(),
                        new Class<?>[]{HelloWorld.class},
                        new MyInvocationHandler(new HelloworldImpl()));
        helloWorld.sayHello();
        */
    }

}
```

运行上面的代码，这样一个简单的JDK Proxy就实现了。

### 代理生成过程 <a id="h2_1"></a>

我们之所以天天叫JDK动态代理，是因为这个代理class是由JDK在运行时动态帮我们生成。在解释代理生成过程前，我们先把-Dsun.misc.ProxyGenerator.saveGeneratedFiles=true 这个参数加入到JVM 启动参数中，它的作用是帮我们把JDK动态生成的proxy class 的字节码保存到硬盘中，帮助我们查看具体生成proxy的内容。我用的**Intellij IDEA ,代理class生成后直接放在项目的根目录**下的，以具体的包名为目录结构。

代理类生成的过程主要包括两部分：

* 代理类字节码生成
* 把字节码通过传入的类加载器加载到虚拟机中

Proxy类的getProxyClass方法入口：需要传入类加载器和interface ![&#x8F93;&#x5165;&#x56FE;&#x7247;&#x8BF4;&#x660E;](https://static.oschina.net/uploads/img/201612/23220514_ep4L.png)

然后调用getProxyClass0方法，里面的注解解释很清楚，如果实现当前接口的代理类存在，直接从缓存中返回，如果不存在，则通过ProxyClassFactory来创建。这里可以明显看到有对interface接口数量的限制，不能超过65535。其中proxyClassCache具体初始化信息如下：

proxyClassCache = new WeakCache&lt;&gt;\(new KeyFactory\(\), new ProxyClassFactory\(\)\);  
其中创建代理类的具体逻辑是通过ProxyClassFactory的apply方法来创建的。

![&#x8F93;&#x5165;&#x56FE;&#x7247;&#x8BF4;&#x660E;](https://static.oschina.net/uploads/img/201612/23220548_nstB.png)

ProxyClassFactory里的逻辑包括了包名的创建逻辑，调用ProxyGenerator. generateProxyClass生成代理类，把代理类字节码加载到JVM。

1. 包名生成逻辑默认是com.sun.proxy，如果被代理类是 non-public proxy interface ，则用和被代理类接口一样的包名，类名默认是$Proxy 加上一个自增的整数值。
2. 包名类名准备好后，就是通过ProxyGenerator. generateProxyClass根据具体传入的接口创建代理字节码，-Dsun.misc.ProxyGenerator.saveGeneratedFiles=true 这个参数就是在该方法起到作用，如果为true则保存字节码到磁盘。代理类中，所有的代理方法逻辑都一样都是调用invocationHander的invoke方法，这个我们可以看后面具体代理反编译结果。
3. 把字节码通过传入的类加载器加载到JVM中: defineClass0\(loader, proxyName,proxyClassFile, 0, proxyClassFile.length\);。

```text
 private static final class ProxyClassFactory
        implements BiFunction<ClassLoader, Class<?>[], Class<?>>
    {
        // prefix for all proxy class names
        private static final String proxyClassNamePrefix = "$Proxy";

        // next number to use for generation of unique proxy class names
        private static final AtomicLong nextUniqueNumber = new AtomicLong();

        @Override
        public Class<?> apply(ClassLoader loader, Class<?>[] interfaces) {

            Map<Class<?>, Boolean> interfaceSet = new IdentityHashMap<>(interfaces.length);
            for (Class<?> intf : interfaces) {
                /*
                 * Verify that the class loader resolves the name of this
                 * interface to the same Class object.
                 */
                Class<?> interfaceClass = null;
                try {
                    interfaceClass = Class.forName(intf.getName(), false, loader);
                } catch (ClassNotFoundException e) {
                }
                if (interfaceClass != intf) {
                    throw new IllegalArgumentException(
                        intf + " is not visible from class loader");
                }
                /*
                 * Verify that the Class object actually represents an
                 * interface.
                 */
                if (!interfaceClass.isInterface()) {
                    throw new IllegalArgumentException(
                        interfaceClass.getName() + " is not an interface");
                }
                /*
                 * Verify that this interface is not a duplicate.
                 */
                if (interfaceSet.put(interfaceClass, Boolean.TRUE) != null) {
                    throw new IllegalArgumentException(
                        "repeated interface: " + interfaceClass.getName());
                }
            }

            String proxyPkg = null;     // package to define proxy class in
            int accessFlags = Modifier.PUBLIC | Modifier.FINAL;

            /*
             * Record the package of a non-public proxy interface so that the
             * proxy class will be defined in the same package.  Verify that
             * all non-public proxy interfaces are in the same package.
             */
           //生成包名和类名逻辑
            for (Class<?> intf : interfaces) {
                int flags = intf.getModifiers();
                if (!Modifier.isPublic(flags)) {
                    accessFlags = Modifier.FINAL;
                    String name = intf.getName();
                    int n = name.lastIndexOf('.');
                    String pkg = ((n == -1) ? "" : name.substring(0, n + 1));
                    if (proxyPkg == null) {
                        proxyPkg = pkg;
                    } else if (!pkg.equals(proxyPkg)) {
                        throw new IllegalArgumentException(
                            "non-public interfaces from different packages");
                    }
                }
            }

            if (proxyPkg == null) {
                // if no non-public proxy interfaces, use com.sun.proxy package
                proxyPkg = ReflectUtil.PROXY_PACKAGE + ".";
            }

            /*
             * Choose a name for the proxy class to generate.
             */
            long num = nextUniqueNumber.getAndIncrement();
            String proxyName = proxyPkg + proxyClassNamePrefix + num;

            /*
             * Generate the specified proxy class. 生成代理类的字节码
             * -Dsun.misc.ProxyGenerator.saveGeneratedFiles=true 在该部起作用
             */
            byte[] proxyClassFile = ProxyGenerator.generateProxyClass(
                proxyName, interfaces, accessFlags);
            try {
                //加载到JVM中
                return defineClass0(loader, proxyName,
                                    proxyClassFile, 0, proxyClassFile.length);
            } catch (ClassFormatError e) {
                /*
                 * A ClassFormatError here means that (barring bugs in the
                 * proxy class generation code) there was some other
                 * invalid aspect of the arguments supplied to the proxy
                 * class creation (such as virtual machine limitations
                 * exceeded).
                 */
                throw new IllegalArgumentException(e.toString());
            }
        }
    }
```

我们可以根据代理类的字节码进行反编译，可以得到如下结果，其中HelloWorld只有sayHello方法，但是代理类中有四个方法 包括了Object上的三个方法：equals,toString,hashCode。

代理的大概结构包括4部分：

* 静态字段：被代理的接口所有方法都有一个对应的静态方法变量；
* 静态块：主要是通过反射初始化静态方法变量；
* 具体每个代理方法：逻辑都差不多就是 h.invoke，主要是调用我们定义好的invocatinoHandler逻辑,触发目标对象target上对应的方法;
* 构造函数：从这里传入我们InvocationHandler逻辑；

```text
package com.sun.proxy;

import com.yao.HelloWorld;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.lang.reflect.UndeclaredThrowableException;

public final class $Proxy0 extends Proxy implements HelloWorld {
    private static Method m1;
    private static Method m3;
    private static Method m2;
    private static Method m0;

    public $Proxy0(InvocationHandler var1) throws  {
        super(var1);
    }

    public final boolean equals(Object var1) throws  {
        try {
            return ((Boolean)super.h.invoke(this, m1, new Object[]{var1})).booleanValue();
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final void sayHello() throws  {
        try {
            super.h.invoke(this, m3, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final String toString() throws  {
        try {
            return (String)super.h.invoke(this, m2, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final int hashCode() throws  {
        try {
            return ((Integer)super.h.invoke(this, m0, (Object[])null)).intValue();
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    static {
        try {
            m1 = Class.forName("java.lang.Object").getMethod("equals", new Class[]{Class.forName("java.lang.Object")});
            m3 = Class.forName("com.yao.HelloWorld").getMethod("sayHello", new Class[0]);
            m2 = Class.forName("java.lang.Object").getMethod("toString", new Class[0]);
            m0 = Class.forName("java.lang.Object").getMethod("hashCode", new Class[0]);
        } catch (NoSuchMethodException var2) {
            throw new NoSuchMethodError(var2.getMessage());
        } catch (ClassNotFoundException var3) {
            throw new NoClassDefFoundError(var3.getMessage());
        }
    }
}
```

\#\#常见问题：

1. toString\(\) hashCode\(\) equal\(\)方法 调用逻辑：这个三个Object上的方法，如果被调用将和其他接口方法方法处理逻辑一样，都会经过invocationHandler逻辑，从上面的字节码结果就可以明显看出。其他Object上的方法将不会走代理处理逻辑，直接走Proxy继承的Object上方法逻辑。
2. interface 含有equals,toString hashCode方法时，和处理普通接口方法一样，都会走invocation handler逻辑，以目标对象重写的逻辑为准去触发方法逻辑；
3. interface含有重复的方法签名,以接口传入顺序为准，谁在前面就用谁的方法，代理类中只会保留一个，不会有重复的方法签名；

