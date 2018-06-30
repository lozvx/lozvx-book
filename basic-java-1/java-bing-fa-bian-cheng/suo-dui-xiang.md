# 锁对象

[http://thinkinjava.cn/article/36](http://thinkinjava.cn/article/36)

**公平锁和非公平锁**

大多数情况下，为了效率，锁都是不公平的。系统在选择锁的时候都是随机的，不会按照某种顺序，比如时间顺序，公平锁的一大特点：他不会产生饥饿现象。只要你排队 ，最终还是可以得到资源的。如果我们使用 synchronized ，得到的锁就是不公平的。因此，这也是重入锁比 synchronized 强大的一个优势。我们同样写个例子：

ReentrantLock类： 可重入锁

```java
myLock.lock();// a ReentranntLock object
try{
// logic
}finnaly{
myLock.unlock();// make sure the lock is unlocked even if an exception is thrown
}
```

条件对象

每个对象都有一个内部锁，并且该锁有一个内部条件。由锁来管理那些试图进入synchroized方法的线程，由条件来管理那些调用wait的线程。

```java
public synchronized void method()
{
 // logic
}
```

等价于

```java
public void method()
{
this.intrinsicLock.lock();
try
{
//  logic
}
finally
{
this.intrinsicLock.unlock();
}

}
```

调用wait或notifyAll等价于

```text
intrinsicCondition.await();
intrinsicCondition.signalAll();
```

volatile域：

volatile关键字为实例域的同步访问提供你一种免锁机制。如果声明一个域为volatile，那么编译器和虚拟机就知道该域是可能被另一个线程并发更新的。每次从内存中获取。但是volatile变量不能提供原子性。

```text
public void flipDone(){done=!done;}// not atomic
```

读写锁：

ReentrantReadWriteLock。如果很多线程从一个数据结构读取数据而很少线程修改其中数据的话，会十分有用。

readLock：得到一个可以被多个读操作公用的读锁，但会排斥所有写操作。

writeLock：得到一个写锁，排斥所有其它的读操作和写操作。

```java
// 1 构造一个ReentrantReadWriteLock对象
private ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();

// 2 抽取读锁和写锁
private Lock readLock = rwl.readLock();
private Lock writeLock = rwl.writeLock();

// 3 对所有的访问者加读锁
public double  read()
{
readLock.lock();
try{
}finally{readLock.unlock();}
}

// 4 对所有的修改者加写锁
public void write(double 11)
{
writeLock.lock();
try{
}finally{writeLock.unlock();}
}
```

生产者消费者问题可以用阻塞队列实现。当队列已满，生产者线程就会被阻塞，当队列为空，消费者线程就会被阻塞。

Callable 与Future

Runnable封装一个异步运行的任务，可以把它想象成一个没有参数和返回值的异步方法。Callable与Runnable类似，但是有返回值。Callable接口是一个参数化的类型，只有一个方法call。

```java
public interface Callable<V>
{
V call() throws Exception;
}
```

Future保存异步计算的结果。可以启动一个计算，将Future对象交给某个线程，然后忘掉它。Future对象的所有者在结果计算好后就可以获得它。

FutureTask包装器，可以将Callable转换成Future和Runnable，它同时实现这两个接口。

执行器（Executor）类有很多静态工厂方法用来构建线程池

| 方法 | 描述 |
| :--- | :--- |
| newCachedThreadPool | 必要时创建新线程；空闲线程会被保留60秒 |
| newFixedThreadPool | 该池包含固定数量的线程；空闲线程会一直被保留 |
| newSingleThreadExecutor | 只有一个线程的池，该线程顺序执行每一个提交的任务 |
| newScheduledThreadPool | 用于预定执行而构建的固定线程池，替代java.util.Timer |
| newSingleThreadScheduledExecutor | 用于预定执行而构建的单线程池 |

