# LRU缓存算法

LRU， Least Recently Used 近期最少使用算法， 常应用于缓存中的数据淘汰， 其核心思想是“如果数据最近被访问过，那么将来被访问的几率也更高“。  


参考：[http://dongyado.com/algorithm/funny/2016/04/23/implement-lru-cache-by-yourself/](http://dongyado.com/algorithm/funny/2016/04/23/implement-lru-cache-by-yourself/)



CPU缓存：在L1缓存中通过LRU策略逐出的数据被同步到L2，在L2中逐出的数据被同步到L3，在L3逐出的写入的数据则被同步到内存。

volatile修饰的变量，每次变更都直接同步到内存中，这样其他线程就能马上看到变化。



常用的分布式缓存包括Redis,Memcached和阿里巴巴的Tair。因为Redis提供的数据结构比较丰富且简单易用，所以Redis的使用广泛。



