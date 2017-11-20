參考：[/blog.csdn.net/justdo\_it/article/details/2042559](/blog.csdn.net/justdo_it/article/details/2042559)

![](/assets/3133267-3940f043c77c3ed0.png)

![](/assets/java-string-pool.jpeg)

```java
package basic;

public class StringTest {

    public static void main(String[] args) {


        String s1 = new String("abc");

        String s2 = "abc";

        String s3 = new String("abc");



        System.out.println(s1 == s2);

        System.out.println(s1 == s3);

        System.out.println(s2 == s3);
    }
}
```

问题：

1、String s1 =**new**String\("abc"\);请问这条语句执行后会产生几个对象？

2、String s2 ="abc";请问这条语句执行后会产生几个对象？

3、String s3 =**new**String\("abc"\);请问这条语句执行后会产生几个对象？

4、最后输出的内容分别是什么？

哈哈，你的答案是什么呢？

第一道题：

String s1 =**new**String\("abc"\);执行这条语句后会产生两个对象！

⑴这条语句肯定会在堆上分配一个对象。

⑵关键是另一个对象，String对象是不可变的对象，在程序运行的过程中可能用到多个具有相同值得String对象，虚拟机使用string pool来优化这种情况，当有新的String对象要创建的时候，虚拟机首先会检查pool中是否有相同值的String对象，如果有就把这个对象的引用传递给新建立的对象，如果没有，就会创建一个对象并把它放进pool中，再将引用返回。那在执行这条语句的时候pool中肯定是的，所以一定会创建一个对象，并将它放入池中，并将引用返回。

再看这条语句，实际上它调用的是public **String**\([String](http://writeblog.csdn.net/Editor/FCKeditor/相关技术/JDK5/api/java/lang/String.html) original\)这个构造函数，用一个字符串对象去构造另一个字符串对象，没错其实构造出的对象就是original这个对象的拷贝。

第二道题：答案是没有对象产生！

有了第一道题的解释，这道题就变得简单的多！因为在执行了第一条语句后，string pool中已经包含了该对象,所以不会创建对象，只是将string pool中相应对象的引用返回。

第三道题：会产生一个对象

在JAVA中有new关键字就一定会在堆上分配一个对象。

第四道题：输出全是false

s1指向的是在堆上分配的一个对象，s2指向string pool中对象，s3也是指向堆上分配的对象，但这个对象是不同于s1的。

