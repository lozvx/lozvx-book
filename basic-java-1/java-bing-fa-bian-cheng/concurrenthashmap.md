# ConcurrentHashMap

参考：

[http://blog.csdn.net/u010723709/article/details/48007881](http://blog.csdn.net/u010723709/article/details/48007881)

Java1.6版本使用了分段加锁的原理，java1.8则实现的线程安全则是大量使用了CAS算法

ConcurrentHashMap不允许key或value为null值

