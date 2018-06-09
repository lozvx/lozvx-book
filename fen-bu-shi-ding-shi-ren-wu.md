# 分布式定时任务

分布式定时任务原理中最重要的是分布式锁，而分布式锁是控制分布式系统之间同步访问共享资源的一种方式，在分布式系统中常常需要协调它们的动作。

分布式锁的3种实现方式

1）基于数据库实现

依靠数据库的唯一索引来实现。

2）基于Redis的实现

基于Redis的setnx命令实现的。当缓存中的key不存在时，才会设置成功，并且返回true，否则直接返回false。如果返回true，则表示获取到了锁，否则获取锁失败。为了防止死锁，我们还会使用expire命令对key设置一个超时时间。

参考：[https://redis.io/topics/distlock\#distributed-locks-with-redis](https://redis.io/topics/distlock#distributed-locks-with-redis)

3）基于zookeeper的实现。

zookeeper是一个为分布式应用trigonometry一致性服务的开源组件，它内部是一个分层的文件系统目录树结构，规定在同一目录下稚嫩有唯一文件名。

