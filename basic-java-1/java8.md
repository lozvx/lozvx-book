# Java8

{% embed url="https://zhuanlan.zhihu.com/p/28093333" %}



**Lambda**

```text
expression = (variable) -> action

```



```text
Runnable r = () -> System.out.println("do something.");
```

### 函数式接口

函数式接口是只有一个方法的接口，用作lambda表达式的类型。前面写的例子就是一个函数式接口，来看看jdk中的**Runnable**源码  


```text
@FunctionalInterface
public interface Runnable {
    /**
     * When an object implementing interface <code>Runnable</code> is used
     * to create a thread, starting the thread causes the object's
     * <code>run</code> method to be called in that separately executing
     * thread.
     * <p>
     * The general contract of the method <code>run</code> is that it may
     * take any action whatsoever.
     *
     * @see     java.lang.Thread#run()
     */
    public abstract void run();
}
```

JDK默认提供了一套函数式接口。在java.util.function包下。可以分成下面这么几类。

![](../.gitbook/assets/image%20%2819%29.png)

 函数式的准则是，被称为“函数式”的函数或方法都只能修改局部变量，除此之外，它引用的对象都应该是**final**的。  
所有的引用类型字段都指向不可变对象。



 Java 8 允许你通过 **::** 关键字获取方法或者构造函数的的引用



```text
Converter<String, Integer> converter = (from) -> Integer.valueOf(from);
Integer converted = converter.convert("123");
System.out.println(converted); // 123
```

等同于

```text
Converter<String, Integer> converter = Integer::valueOf;
Integer converted = converter.convert("123");
System.out.println(converted); // 123
```

