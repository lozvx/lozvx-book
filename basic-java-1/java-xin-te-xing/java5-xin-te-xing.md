# Java5新特性

* 泛型
* 枚举类型
* 自动装箱拆箱
* 可变参数
* Annotation
* 新的迭代语句 for each

```java
for(String s :list){
}
```

* 静态导入

```java
import static java.lang.System.out;//导入java.lang包下的System类的静态方法out;
public class HelloWorld{
    public static void main(String[] args){
        out.print("Hello World!");//既是在这里不用再写成System.out.println("Hello World!")了，因为已经导入了这个静态方法out。
    }
}
```

* 新的格式化方法（java.util.Formatter）

```text
formatter.format("Remaining account balance: $%.2f",balance);
```

* 新的线程模型和并发库

引入java.util.concurrent包

HashMap -&gt; ConcurrentHashMap

ArrayList -&gt; CopyOnWriteArrayList

BlockingQueue,Callable,Executor,Semaphore....

