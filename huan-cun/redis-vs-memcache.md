# Redis Vs Memcache

### 1.数据类型

Redis支持5种数据类型。

Memcache只支持对键值对的存储，不支持其他的数据类型。

### 2.线程模型

Redis使用单线程实现。

Memcache使用多线程实现.

因此不推荐 在Redis中存储太大的内容，否则会阻塞其他请求。

两者都是使用了非阻塞IO模型。

### 3.持久机制

Redis提供了两种持久机制，包括RDB和AOF，前者是定时的持久机制，但是在出行宕机时肯恩出行数据丢失，后者是基于操作日志的持久机制。

Memcache并不提供持久机制，因为memcache的设计理念就是设计一个单纯的缓存，缓存的数据都是临时的，不应该是持久的，也不应该是一个大数据的数据库，缓存未命中时回源查找数据库是天经地义的。

### 4.客户端

常见的redis Java客户端Jedis使用阻塞IO，但是可以配置连接池，并提供了一致性哈希分片的逻辑，也可以使用开元的客户端分片框架Redic。

Memcache的客户端包括Memcache Java Client，Spy Client， XMemcache等。Memcache Java Client使用阻塞IO，而Spy CLient和XMemcache使用非阻塞IO。

### 5.高可用

Redis支持主从节点复制配置，从节点可使用RDB和缓存的AOF命令进行同步和恢复。Redis还支持Sentiel和Cluster等高可用集群方案。

Memcache不支持高可用模型，可使用第三方megagent代理，当一个视力宕机时，可以连接另一个实例来实现。

### 6.对队列的支持

Redis支持lpush/brpop,publish/subscribe/psubscribe等队列和订阅模式

Memcache不支持队列。

### 7.事务

Redis提供了Multi/exec命令。由于Redis服务器是单线程的，任何球球的服务器操作命令都是原子的，但是跨客户端的操作并不保证原子性，所以对同一连接的多个操作序列也不保证事务。

Memcache的单个命令也是线程安全的。

### 8.数据淘汰策略

Redis提供丰富的淘汰策略，包括maxmemory，maxmemory-policy，volatile-lru，allkey-lru，volatile-ramdom，allkeys-random，volatile-ttl，noevication等。

memcache在容量达到指定值后，就基于LRU算法自动删除不适用的缓存。当然，可以通过参数禁止LRU算法。

### 9.内存分配

....





