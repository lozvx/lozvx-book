参考：

http://blog.csdn.net/javazejian/article/details/77410889



阻塞队列与我们平常接触的普通队列\(LinkedList或ArrayList等\)的最大不同点，在于阻塞队列支出阻塞添加和阻塞删除方法。

* 阻塞添加  
  所谓的阻塞添加是指当阻塞队列元素已满时，队列会阻塞加入元素的线程，直队列元素不满时才重新唤醒线程执行元素加入操作。

* 阻塞删除  
  阻塞删除是指在队列元素为空时，删除队列元素的线程将被阻塞，直到队列不为空再执行删除操作\(一般都会返回被删除的元素\)

由于Java中的阻塞





有点需要注意的是ArrayBlockingQueue内部的阻塞队列是通过重入锁ReenterLock和Condition条件队列实现的，所以ArrayBlockingQueue中的元素存在公平访问与非公平访问的区别，对于公平访问队列，被阻塞的线程可以按照阻塞的先后顺序访问队列，即先阻塞的线程先访问队列。而非公平队列，当队列可用时，阻塞的线程将进入争夺访问资源的竞争中，也就是说谁先抢到谁就执行，没有固定的先后顺序。创建公平与非公平阻塞队列代码如下：

```
//默认非公平阻塞队列
ArrayBlockingQueue queue = new ArrayBlockingQueue(2);
//公平阻塞队列
ArrayBlockingQueue queue1 = new ArrayBlockingQueue(2,true);

//构造方法源码
public ArrayBlockingQueue(int capacity) {
     this(capacity, false);
 }

public ArrayBlockingQueue(int capacity, boolean fair) {
     if (capacity <= 0)
         throw new IllegalArgumentException();
     this.items = new Object[capacity];
     lock = new ReentrantLock(fair);
     notEmpty = lock.newCondition();
     notFull =  lock.newCondition();
 }
```



