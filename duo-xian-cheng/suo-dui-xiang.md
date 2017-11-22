ReentrantLock类： 可重入锁

```java
myLock.lock();// a ReentranntLock object
try{
// logic
}finnaly{
myLock.unlock();// make sure the lock is unlocked even if an exception is thrown
}
```

---

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

```
intrinsicCondition.await();
intrinsicCondition.signalAll();
```

---

volatile域：

volatile关键字为实例域的同步访问提供你一种免锁机制。如果声明一个域为volatile，那么编译器和虚拟机就知道该域是可能被另一个线程并发更新的。每次从内存中获取。但是volatile变量不能提供原子性。

```
public void flipDone(){done=!done;}// not atomic
```

---

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



