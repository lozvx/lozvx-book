# GC

 GC算法 

Serial 

单线程堆进行堆清理

 Minor GC 和 Full GC时都会导致应用线程停止

 Full GC 对空间对象进行压缩 



Throughput/Parallel 

多线程进行堆清理

 Minor GC 和Full GC 会快很多，依然导致应用线程停止

 -XX:+UseParallelGC\(新生代\) -XX:+UseParallelOldGC（老年代）

 CMS

 Minor GC 处理方式与Throughput相同，应用线程停止

 多线程并发对Old Generation，进行定期扫描和清理. 次数频繁，依然会有停顿，但是单次停顿时间短

 CPU消耗大，同时会造成大量内存碎片

 如果堆内存过度碎片化，会导致蜕化为Serial 单线程完成对Old Generation 内存整理，停顿时间更长 

-XX:+UseParNewGC \(新生代\) -XX:+UseConcMarkSweepGC （老年代）

 G1 

目的是减少超大堆的停顿（&gt;4G）

 将堆分为若干个区域\(Region\), 新老代都会处于分片中

 对比CMS而言，对堆进行分片，老年代分片之间进行移动，相当于内存压缩，减少了内存碎片

 新生带处理方式同其他，多线程，应用线程停止

 -XX:+UseG1GC



 问题1 : 为什么会有stop-the-world?

 GC 线程回收对象，或者移动对象时，对象地址发生变化 应用线程得不到正确的对象地址 

问题2 : 为什么 Minor GC 不会非常影响性能

／目前所有的GC算法优化都集中在Full GC上 新生代仅为整个堆的一部分，新生对象生成和失效频率和速度都较高 高频收集-&gt; 应用线程频繁停顿 -&gt;单次收集数量少- &gt; 应用线程停顿时间短 对象分配至eden, 未被回收的对象去向为survivor 或者old generation，每次都被清空，相当于一次对象空间压缩 因此，基于分代的GC，在MinorGC上效率比较高

