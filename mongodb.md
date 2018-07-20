# MongoDB

Mongo DB  
  
  
http://www.runoob.com/mongodb/mongodb-tutorial.html  
  
  
\#\# 什么是MongoDB ?  
MongoDB 是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。  
在高负载的情况下，添加更多的节点，可以保证服务器性能。  
MongoDB 旨在为WEB应用提供可扩展的高性能数据存储解决方案。  
MongoDB 将数据存储为一个文档，数据结构由键值\(key=&gt;value\)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。  
  
  
\| SQL术语/概念 \| MongoDB术语/概念 \| 解释/说明 \|\| ----------- \| ------------ \| ----------------------- \|\| database \| database \| 数据库 \|\| table \| collection \| 数据库表/集合 \|\| row \| document \| 数据记录行/文档 \|\| column \| field \| 数据字段/域 \|\| index \| index \| 索引 \|\| table joins \| \| 表连接,MongoDB不支持 \|\| primary key \| primary key \| 主键,MongoDB自动将\_id字段设置为主键 \|  
  
  
\#\#\#应用场景：  
https://www.zhihu.com/question/32071167  
http://wiki.jikexueyuan.com/project/the-little-mongodb-book/when-use-mongodb.html



\#\# NoSQL 数据库分类  
\| 类型 \| 部分代表 \| 特点 \|\| ----------- \| ---------------------------------------- \| ---------------------------------------- \|\| 列存储 \| HbaseCassandraHypertable \| 顾名思义，是按列存储数据的。最大的特点是方便存储结构化和半结构化数据，方便做数据压缩，对针对某一列或者某几列的查询有非常大的IO优势。 \|\| 文档存储 \| MongoDBCouchDB \| 文档存储一般用类似json的格式存储，存储的内容是文档型的。这样也就有有机会对某些字段建立索引，实现关系数据库的某些功能。 \|\| key-value存储 \| Tokyo Cabinet / TyrantBerkeley DBMemcacheDBRedis \| 可以通过key快速查询到其value。一般来说，存储不管value的格式，照单全收。（Redis包含了其他功能） \|\| 图存储 \| Neo4JFlockDB \| 图形关系的最佳存储。使用传统关系数据库来解决的话性能低下，而且设计使用不方便。 \|\| 对象存储 \| db4oVersant \| 通过类似面向对象语言的语法操作数据库，通过对象的方式存取数据。 \|\| xml数据库 \| Berkeley DB XMLBaseX \| 高效的存储XML数据，并支持XML的内部查询语法，比如XQuery,Xpath。 \|

