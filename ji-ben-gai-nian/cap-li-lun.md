# CAP理论

[http://www.hollischuang.com/archives/666](http://www.hollischuang.com/archives/666)

{% embed data="{\"url\":\"https://zh.wikipedia.org/zh-hans/CAP%E5%AE%9A%E7%90%86\",\"type\":\"link\",\"title\":\"CAP定理 - 维基百科，自由的百科全书\",\"icon\":{\"type\":\"icon\",\"url\":\"https://zh.wikipedia.org/static/apple-touch/wikipedia.png\",\"aspectRatio\":0}}" %}

在[理论计算机科学](https://zh.wikipedia.org/wiki/%E7%90%86%E8%AB%96%E8%A8%88%E7%AE%97%E6%A9%9F%E7%A7%91%E5%AD%B8)中，**CAP定理**（CAP theorem），又被称作**布鲁尔定理**（Brewer's theorem），它指出对于一个[分布式计算系统](https://zh.wikipedia.org/wiki/%E5%88%86%E5%B8%83%E5%BC%8F%E8%AE%A1%E7%AE%97)来说，不可能同时满足以下三点：[\[1\]](https://zh.wikipedia.org/wiki/CAP%E5%AE%9A%E7%90%86#cite_note-Lynch-1)[\[2\]](https://zh.wikipedia.org/wiki/CAP%E5%AE%9A%E7%90%86#cite_note-2)

* 一致性（**C**onsistence） （等同于所有节点访问同一份最新的数据副本）
* 可用性（[**A**vailability](https://zh.wikipedia.org/wiki/%E5%8F%AF%E7%94%A8%E6%80%A7)）（每次请求都能获取到非错的响应——但是不保证获取的数据为最新数据）
* 分区容错性（[Partition tolerance](https://zh.wikipedia.org/w/index.php?title=Partition_tolerance&action=edit&redlink=1)）（以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在C和A之间做出选择[\[3\]](https://zh.wikipedia.org/wiki/CAP%E5%AE%9A%E7%90%86#cite_note-3)。）

根据定理，分布式系统只能满足三项中的两项而不可能满足全部三项[\[4\]](https://zh.wikipedia.org/wiki/CAP%E5%AE%9A%E7%90%86#cite_note-4)。理解CAP理论的最简单方式是想象两个节点分处分区两侧。允许至少一个节点更新状态会导致数据不一致，即丧失了C性质。如果为了保证数据一致性，将分区一侧的节点设置为不可用，那么又丧失了A性质。除非两个节点可以互相通信，才能既保证C又保证A，这又会导致丧失P性质。



著名的CAP理论指出，一个分布式系统不可能同时满足C\(一致性\)、A\(可用性\)和P\(分区容错性\)。由于分区容错性在是分布式系统中必须要保证的，因此我们只能在A和C之间进行权衡。在此Zookeeper保证的是CP, 而Eureka则是AP。

### 4.1 Zookeeper保证CP {#41-zookeeper-bao-zheng-cp}

当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的注册信息，但不能接受服务直接down掉不可用。也就是说，服务注册功能对可用性的要求要高于一致性。但是zk会出现这样一种情况，当master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举。问题在于，选举leader的时间太长，30 ~ 120s, 且选举期间整个zk集群都是不可用的，这就导致在选举期间注册服务瘫痪。在云部署的环境下，因网络问题使得zk集群失去master节点是较大概率会发生的事，虽然服务能够最终恢复，但是漫长的选举时间导致的注册长期不可用是不能容忍的。

### 4.2 Eureka保证AP {#42-eureka-bao-zheng-ap}

Eureka看明白了这一点，因此在设计时就优先保证可用性。Eureka各个节点都是平等的，几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务。而Eureka的客户端在向某个Eureka注册或时如果发现连接失败，则会自动切换至其它节点，只要有一台Eureka还在，就能保证注册服务可用\(保证可用性\)，只不过查到的信息可能不是最新的\(不保证强一致性\)。除此之外，Eureka还有一种自我保护机制，如果在15分钟内超过85%的节点都没有正常的心跳，那么Eureka就认为客户端与注册中心出现了网络故障，此时会出现以下几种情况： 1. Eureka不再从注册列表中移除因为长时间没收到心跳而应该过期的服务 2. Eureka仍然能够接受新服务的注册和查询请求，但是不会被同步到其它节点上\(即保证当前节点依然可用\) 3. 当网络稳定时，当前实例新的注册信息会被同步到其它节点中

因此， Eureka可以很好的应对因网络故障导致部分节点失去联系的情况，而不会像zookeeper那样使整个注册服务瘫痪。

##   {#5-zong-jie}



