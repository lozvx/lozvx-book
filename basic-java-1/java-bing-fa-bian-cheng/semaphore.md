# Semaphore



信号量\(Semaphore\)，又被称为信号灯，在多线程环境下用于协调各个线程, 以保证它们能够正确、合理的使用公共资源。信号量维护了一个许可集，我们在初始化Semaphore时需要为这个许可集传入一个数量值，该数量值代表同一时间能访问共享资源的线程数量。线程可以通过`acquire()`方法获取到一个许可，然后对共享资源进行操作，注意如果许可集已分配完了，那么线程将进入等待状态，直到其他线程释放许可才有机会再获取许可，线程释放一个许可通过`release()`方法完成。下面通过一个简单案例来演示

```text
public class SemaphoreTest {

    public static void main(String[] args) {  
       // 线程池 
       ExecutorService exec = Executors.newCachedThreadPool();  
       //设置信号量同时执行的线程数是5 
       final Semaphore semp = new Semaphore(5);  
       // 模拟20个客户端访问 
       for (int index = 0; index < 20; index++) {
           final int NO = index;  
           Runnable run = new Runnable() {  
               public void run() {  
                   try {  
                       //使用acquire()获取锁 
                       semp.acquire();  
                       System.out.println("Accessing: " + NO);  
                       //睡眠1秒
                       Thread.sleep(1000);  

                   } catch (InterruptedException e) {  
                   }  finally {
                        //使用完成释放锁 
                        semp.release();
                    }
               }  
           };  
           exec.execute(run);  
       }  
       // 退出线程池 
       exec.shutdown();  
   }  
}
```

 

在AQS中存在一个变量state，当我们创建Semaphore对象传入许可数值时，最终会赋值给state，state的数值代表同一个时刻可同时操作共享数据的线程数量，每当一个线程请求\(如调用Semaphored的acquire\(\)方法\)获取同步状态成功，state的值将会减少1，直到state为0时，表示已没有可用的许可数，也就是对共享数据进行操作的线程数已达到最大值，其他后来线程将被阻塞，此时AQS内部会将线程封装成共享模式的Node结点，加入同步队列中等待并开启自旋操作。只有当持有对共享数据访问权限的线程执行完成任务并释放同步状态后，同步队列中的对于的结点线程才有可能获取同步状态并被唤醒执行同步操作，注意在同步队列中获取到同步状态的结点将被设置成head并清空相关线程数据\(毕竟线程已在执行也就没有必要保存信息了\)，AQS通过这种方式便实现共享锁



 对共享锁的实现有了基本的认识，即AQS中通过state值来控制对共享资源访问的线程数，每当线程请求同步状态成功，state值将会减1，如果超过限制数量的线程将被封装共享模式的Node结点加入同步队列等待，直到其他执行线程释放同步状态，才有机会获得执行权，而每个线程执行完成任务释放同步状态后，state值将会增加1，这就是共享锁的基本实现模型。至于公平锁与非公平锁的不同之处在于公平锁会在线程请求同步状态前，判断同步队列是否存在Node，如果存在就将请求线程封装成Node结点加入同步队列，从而保证每个线程获取同步状态都是先到先得的顺序执行的。非公平锁则是通过竞争的方式获取，不管同步队列是否存在Node结点，只有通过竞争获取就可以获取线程执行权。

