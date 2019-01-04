# 第五章 创建高性能的索引

MySQL支持的索引

B-Tree索引

InnoDB使用B+树结构存储索引，根据主键引用被索引的行。

例：

```text
create table people(
lastname varchar(50) not null,
firstname varchat(50) not null,
birthday date not null,
gender enum('m','f') not null,
key(lastname,firstname,birthday)
);
```

全值匹配： 指的是和索引中的所有列进行匹配。

匹配最左前缀：只使用索引的第一列或第一第二列

匹配列前缀：匹配某一列的值的开头部分

匹配范围值：查找姓在allen和bob之间的人

精确匹配某一列并范围匹配另外一列：

只访问索引的查询：查询只需要访问索引，而 无须访问数据行。



因为索引树的节点是有序的，所以索引还可以用于查询中的order by操作。

B-Tree索引的限制：

如果不是按照索引的最左列开始查找，则无法使用索引

不能跳过索引中的列，如只用lastname birthday查询

如果查询中有某个列的查询访问，则其右边的所有列都无法使用索引优化查找



