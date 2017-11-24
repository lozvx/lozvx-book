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

```
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

因此在synchronized中再进行一次\(instance	==	null\)判断,这种方式称为双重检查锁定\(Double-CheckLocking\)

```
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



