https://github.com/huangz1990/annotated\_redis\_source

http://docs.spring.io/spring-data/redis/docs/current/reference/html/

http://www.cnblogs.com/edwinchen/p/3816938.html

http://redis.io/topics/quickstart

1.《The Little Redis Book》

是一本开源PDF，只有29页的英文文档，看完后对Redis的基本概念应该差不多熟悉了，剩下的可以去Redis官网熟悉相关的命令。

https://github.com/karlseguin/the-little-redis-book

https://www.zybuluo.com/haokuixi/note/26177

2.《Redis设计与实现》

http://redisbook.com/index.html

如果想继续深入，推荐这本书，现在已经出到第二版了，有纸质版书籍可以购买。上面详细介绍了Redis的一些设计理念，并且给出了一些内部实现方式，和数据结构的C语言定义，有一些基本C语言基础，就能看明白。

3.Redis 2.6源代码：

《Redis设计与实现》的作者发布在Github上的一个开源项目，有作者详细的注释，

https://github.com/huangz1990/annotated\_redis\_source

redis 命令：http://redis.io/commands



Redis是用ANSI C写的一个基于内存的Key-Value数据库，而Jedis是Redis官方推出的面向Java的Client，提供了很多接口和方法，可以让Java 操作使用Redis，而Spring Data Redis是对Jedis进行了封装，集成了Jedis的一些命令和方法，可以与Spring整合。



- 字符串（string）、散列（hash）、列表（list）、集合（set）和有序集合（sorted set）这五种类型的键的底层实现数据结构。

