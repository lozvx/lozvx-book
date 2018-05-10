# 原子操作类

原子更新基本类型

* AtomicBoolean：原子更新布尔类型
* AtomicInteger：原子更新整型
* AtomicLong：原子更新长整型

这3个类的实现原理和使用方式几乎是一样的，这里我们以AtomicInteger为例进行分析，AtomicInteger主要是针对int类型的数据执行原子操作，它提供了原子自增方法、原子自减方法以及原子赋值方法等，鉴于AtomicInteger的源码不多，我们直接看源码

```text
public class AtomicInteger extends Number implements java.io.Serializable {
    private static final long serialVersionUID = 6214790243416807050L;

    // 获取指针类Unsafe
    private static final Unsafe unsafe = Unsafe.getUnsafe();

    //下述变量value在AtomicInteger实例对象内的内存偏移量
    private static final long valueOffset;

    static {
        try {
           //通过unsafe类的objectFieldOffset()方法，获取value变量在对象内存中的偏移
           //通过该偏移量valueOffset，unsafe类的内部方法可以获取到变量value对其进行取值或赋值操作
            valueOffset = unsafe.objectFieldOffset
                (AtomicInteger.class.getDeclaredField("value"));
        } catch (Exception ex) { throw new Error(ex); }
    }
   //当前AtomicInteger封装的int变量value
    private volatile int value;

    public AtomicInteger(int initialValue) {
        value = initialValue;
    }
    public AtomicInteger() {
    }
   //获取当前最新值，
    public final int get() {
        return value;
    }
    //设置当前值，具备volatile效果，方法用final修饰是为了更进一步的保证线程安全。
    public final void set(int newValue) {
        value = newValue;
    }
    //最终会设置成newValue，使用该方法后可能导致其他线程在之后的一小段时间内可以获取到旧值，有点类似于延迟加载
    public final void lazySet(int newValue) {
        unsafe.putOrderedInt(this, valueOffset, newValue);
    }
   //设置新值并获取旧值，底层调用的是CAS操作即unsafe.compareAndSwapInt()方法
    public final int getAndSet(int newValue) {
        return unsafe.getAndSetInt(this, valueOffset, newValue);
    }
   //如果当前值为expect，则设置为update(当前值指的是value变量)
    public final boolean compareAndSet(int expect, int update) {
        return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
    }
    //当前值加1返回旧值，底层CAS操作
    public final int getAndIncrement() {
        return unsafe.getAndAddInt(this, valueOffset, 1);
    }
    //当前值减1，返回旧值，底层CAS操作
    public final int getAndDecrement() {
        return unsafe.getAndAddInt(this, valueOffset, -1);
    }
   //当前值增加delta，返回旧值，底层CAS操作
    public final int getAndAdd(int delta) {
        return unsafe.getAndAddInt(this, valueOffset, delta);
    }
    //当前值加1，返回新值，底层CAS操作
    public final int incrementAndGet() {
        return unsafe.getAndAddInt(this, valueOffset, 1) + 1;
    }
    //当前值减1，返回新值，底层CAS操作
    public final int decrementAndGet() {
        return unsafe.getAndAddInt(this, valueOffset, -1) - 1;
    }
   //当前值增加delta，返回新值，底层CAS操作
    public final int addAndGet(int delta) {
        return unsafe.getAndAddInt(this, valueOffset, delta) + delta;
    }
   //省略一些不常用的方法....
}
```

Demo：

```text
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicIntegerDemo {
    //创建AtomicInteger,用于自增操作
    static AtomicInteger i = new AtomicInteger();
    static int i2 = 0;

    public static class AddThread implements Runnable {
        public void run() {
            for (int k = 0; k < 10000; k++)
                i.incrementAndGet();
        }
    }

    public static class AddThread2 implements Runnable {
        public void run() {
            for (int k = 0; k < 10000; k++)
                i2++;
        }

    }

    public static void main(String[] args) throws InterruptedException {
        test1();
        test2();
    }

    private static void test1() throws InterruptedException {
        Thread[] ts = new Thread[10];
        //开启10条线程同时执行i的自增操作
        for (int k = 0; k < 10; k++) {
            ts[k] = new Thread(new AddThread());
        }
        //启动线程
        for (int k = 0; k < 10; k++) {
            ts[k].start();
        }

        for (int k = 0; k < 10; k++) {
            ts[k].join();
        }

        System.out.println(i);//输出结果:100000
    }

    private static void test2() throws InterruptedException {
        Thread[] ts = new Thread[10];
        //开启10条线程同时执行i的自增操作
        for (int k = 0; k < 10; k++) {
            ts[k] = new Thread(new AddThread2());
        }
        //启动线程
        for (int k = 0; k < 10; k++) {
            ts[k].start();
        }

        for (int k = 0; k < 10; k++) {
            ts[k].join();
        }

        System.out.println(i2);//输出结果:100000
    }
}
```

原子更新引用：

```text
public class AtomicReference<V> implements java.io.Serializable {
    private static final Unsafe unsafe = Unsafe.getUnsafe();
    private static final long valueOffset;

    static {
        try {
            valueOffset = unsafe.objectFieldOffset
                (AtomicReference.class.getDeclaredField("value"));
        } catch (Exception ex) { throw new Error(ex); }
    }
    //内部变量value，Unsafe类通过valueOffset内存偏移量即可获取该变量
    private volatile V value;

//CAS方法，间接调用unsafe.compareAndSwapObject(),它是一个
//实现了CAS操作的native方法
public final boolean compareAndSet(V expect, V update) {
        return unsafe.compareAndSwapObject(this, valueOffset, expect, update);
}

//设置并获取旧值
public final V getAndSet(V newValue) {
        return (V)unsafe.getAndSetObject(this, valueOffset, newValue);
    }
    //省略其他代码......
}

//Unsafe类中的getAndSetObject方法，实际调用还是CAS操作
public final Object getAndSetObject(Object o, long offset, Object newValue) {
      Object v;
      do {
          v = getObjectVolatile(o, offset);
      } while (!compareAndSwapObject(o, offset, v, newValue));
      return v;
  }
```

Demo：

```text
public class AtomicReferenceDemo2 {

    public static AtomicReference<User> atomicUserRef = new AtomicReference<User>();

    public static void main(String[] args) {
        User user = new User("zejian", 18);
        atomicUserRef.set(user);
        User updateUser = new User("Shine", 25);
        atomicUserRef.compareAndSet(user, updateUser);
        //执行结果:User{name='Shine', age=25}
              System.out.println(atomicUserRef.get().toString());  
    }

    static class User {
        public String name;
        private int age;

        public User(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public String getName() {
            return name;
        }

        @Override
        public String toString() {
            return "User{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    '}';
        }
    }
}
```

