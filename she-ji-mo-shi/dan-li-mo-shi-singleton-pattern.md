# 单例模式-Singleton Pattern

单例模式\(Singleton Pattern\):确保某一个类只有一个实例,而且自行实例化并向整个系统提供这个实例,这个类称为单例类,它提供全局访问的方法。单例模式是一种对象创建型模式。

* 饿汉式单例

```java
package designpattern.singleton;

public class EagerSingleton {

    private static final EagerSingleton instance = new EagerSingleton();

    private EagerSingleton() {

    }

    public static EagerSingleton getInstance() {
        return instance;

    }
}
```

* 懒汉式单例类与 线程锁定

```text
package designpattern.singleton;

public class LazySingleton {
    private static LazySingleton instance = null;

    private LazySingleton() {
    }

    synchronized public static LazySingleton getInstance() {
        if (instance == nll) {
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

优化版本：

上面的代码因为把synchronized关键字直接加在genInstance方法上，虽然能防止初始化实例时有多线程的问题，但是影响了后续的性能。

因此在synchronized中再进行一次\(instance == null\)判断,这种方式称为双重检查锁定\(Double-CheckLocking\)

```text
package designpattern.singleton;

public class LazySingleton2 {
    private volatile static LazySingleton2 instance = null;

    private LazySingleton2() {
    }

    public static LazySingleton2 getInstance() {
        //第一重判断
        if (instance == null) {
            //锁定代码块
            synchronized (LazySingleton.class) {
                //第二重判断
                if (instance == null) {
                    instance = new LazySingleton2();    //创建单例实例
                }
            }
        }
        return instance;
    }
}
```

饿汉式单例类与懒汉式单例类比较

* 饿汉式单例类在类被加载时就将自己实例化,它的优点在于无须考虑多线程访问问题,可以确保实例的唯一性;从调用速度和反应时间角度来讲,由于单例对象一开始就得以创建,因此要优于懒汉式单例。但是无论系统在运行时是否需要使用该单例对象,由于在类加载时该对象就需要创建,因此从资源利用效率角度来讲,饿汉式单例不及懒汉式单例,而且在系统加载时由于需要创建饿汉式单例对象,加载时间可能会比较长。
* 懒汉式单例类在第一次使用时创建,无须一直占用系统资源,实现了延迟加载,但是必须处理好多个线程同时访问的问题,特别是当单例类作为资源控制器,在实例化时必然涉及资源初始化,而资源初始化很有可能耗费大量时间,这意味着出现多线程同时首次引用此类的机率变得较大,需要通过双重检查锁定等机制进行控制,这将导致系统性能受到一定影响。

