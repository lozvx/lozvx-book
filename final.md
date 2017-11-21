参考：[http://www.cnblogs.com/lwbqqyumidi/p/3513047.html](http://www.cnblogs.com/lwbqqyumidi/p/3513047.html)

final修饰finalfin量时：final修饰变量时final变量时：final修饰变final修饰变量时final量时fin量时：final修饰变量时final变量时：final修饰变final修饰变量时final修饰变量final修饰finalfin量时：final修饰变量时final变量时：final修饰变final修饰变量时final量时fin量时：final修饰变量时final变量时：final修饰变final修饰变量时final修饰变量

* 修饰变量

表示这个变量只能赋值一次，如果修饰的是对象，那引用不能变更，但是对象里的Field还是可能被修改

```java
package basic;

/**
 * Created by desmond on 16-9-21.
 */
public class FinalTest {

    public static void main(String[] args) {
        new FinalTest().testInt(12);
        A a = new A("bb");
        new FinalTest().testFinal2(a);
        System.out.println(a.getContent());
    }

    /*
     * 第一种情况，修饰基本类型（非引用类型）。这时参数的值在方法体内是不能被修改的，即不能被重新赋值。否则编译就通不过。例如：
     */
    public void testInt(final int param1) {
//        param1 = 100;
    }

    /*
     * 第二种情况，修饰引用类型。这时参数变量所引用的对象是不能被改变的。作为引用的拷贝，参数在方法体里面不能再引用新的对象。否则编译通不过。例如：
     */
    public void testFinal2(final A param2) {
//        param2 = new Object();
        param2.setContent("aa");
    }
}
```

* 修饰类

用final修饰的类不能被继承，如果一个类声明为final，其中的方法自动成为final方法

* 修饰方法

用final修饰的方法不能被重写，但是可以重载多个final修饰到方法

