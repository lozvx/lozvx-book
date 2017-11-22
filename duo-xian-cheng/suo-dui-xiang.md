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



