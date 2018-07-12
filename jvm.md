# JVM

JVM的结构基本由4个部分组成

1. 类加载器， 在JVM启动时或者类运行时将需要的class加载到JVM中。
2. 执行引擎，负责执行class文件包含的字节码指令。如sun的hotspot是基于栈的，google的Dalvik是基于寄存器的
3. 内存区， 将内存划分陈若干区域。包括方法区，Java堆，Java栈，PC寄存器和本地方法区。其中方法区和Java堆是所有线程共享的。
4. 本地方法调用，调用c或c++实现的本地方法。



JVM内存管理

![](.gitbook/assets/image%20%285%29.png)

Java8内存模型：

 1.8同1.7比，最大的差别就是：**元数据区取代了永久代**

[https://www.cnblogs.com/paddix/p/5309550.html](https://www.cnblogs.com/paddix/p/5309550.html)

Java堆

Java堆是用于存储Java对象的内存区域，堆的大小在JVM启动时就一次向操作系统申请完成，通过-Xmx和-Xms两个选项来控制大小，Xmx表示堆的最大大小，Xms表示初始大小。

线程

JVM运行实际程序的实体是线程，每个线程创建时JVM都会为它创建一个堆栈，堆栈的大小根据不同的JVM实现而不同，通常在256KB-756KB之间。



类和类加载器

在Java中类和类加载器也被存储在堆中，这个区域叫做永久代。JVM加载类是按需加载的。如果需要查看加载了哪些类，可以在启动参数上加上-verbose:class就会打印出加载了哪些类

```text
Loaded java.lang.Object from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.io.Serializable from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.Comparable from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.CharSequence from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.String from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.reflect.AnnotatedElement from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.reflect.GenericDeclaration from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.reflect.Type from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.Class from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.Cloneable from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.ClassLoader from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.System from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.Throwable from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar]
[Loaded java.lang.Error from C:\Program Files\Java\jdk1.8.0_172\jre\lib\rt.jar
```



堆：

新生代，Young区又分为Eden区和两个Survivor区。

所有新创建的对象都在Eden区，当Eden区满后会触发minor GC，将Eden区仍然存活的对象复制到其中一个Survivor区，另外一个Survivor区中存活对象也复制到这个Survivor区中。以保证始终有一个Survivor区是空的。

老年代，old区

old区存放的是Young区的Survivor满后触发monor GC后仍然存活的对象，当Eden区满后会将对象存放到Survivor区，如果Survivor区仍然存不下这些对象，GC收集器会将这些对象直接存放到old区。如果在Survivor区中的对象足够老，也直接存放到old区。如果old区满了，将会触发Full GC,回收整个堆内存。

永久代，Perm区（永久代不在堆中，1.8已经把永久代删除，替换为元数据，metaspace）

Perm区存放的主要是类的class对象，如果一个类被频繁的加载，也可能会导致Perm区满，perm区的 垃圾回收也由Full GC触发。



