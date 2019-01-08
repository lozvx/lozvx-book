# Mysql 查询性能优化

一般MySQL能够使用如下三种方式应用where条件，从好到坏依次为：

1.在索引中使用where条件来 过滤不匹配的记录。这是在存储引擎层完成的。

2.使用索引覆盖扫描（在Extra列中出现Using Index）来返回记录，直接从索引中过滤不需要的记录并返回命中的结果。这是在MySQL服务器层完成的，当无须再回表查询记录。

3.从数据表中返回数据，然后过滤不满足条件的记录（在Extra列中出现Using Where）。这在MySQL服务器层完成，MySQL需要先从数据库表读出记录然后过滤。



## MySQL 执行计划

1. 语句是否使用索引，使用了怎样的索引
2. 组合索引是否完全使用
3. 执行时大致的扫描行数
4. 是否产生了临时表
5. 是否需要外部排序

### select\_type列

1. SIMPLE——除子查询或union查询之外的其他查询 

2. PRIMARY——子查询中的最外层查询 

3. SUBQUARY——select或where中包含的子查询 

4. DERIVED——from中包含的子查询

 5. UNION——union语句中第二个select开始后面所有的select

### type列 

1. ALL——万恶的全表扫描 

2. INDEX——全索引扫描 

3. RANGE——索引范围扫描 

4. REF——非唯一索引扫描，扫描某个单独值的所有行

 5. EQ\_REF——join中被驱动表匹配字段是主键或唯一非空索引 

6. CONST——主键或唯一索引扫描，只会有一条记录匹配 

7. SYSTEM——const的特殊形式，当表里只有一条记录时



### possible\_keys列

MySQL可能用到哪些索引找到行

### key列 

实际使用的索引，如果没有，则显示为NULL

### key\_len列 

使用的索引长度

### ref列

表示匹配条件，是通过产量还是某个字段来查找行

### rows列

根据表的统计信息来估算出，找到所需记录所需要读取的行数

### Extra列

1. Using where  需要回标取数据
2. Using index 使用了索引覆盖
3. Using temporary 使用了临时表
4. Using filesort 使用文件排序

 



