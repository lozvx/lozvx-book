# 第四章 schema与数据类型优化

日期与时间

DATETIME

这个类型能保存大范围的值，从1001年到9999年，精度为秒。它把日期和时间封装到格式为YYYYMMDDHHMMSS的整数中，与市区无关。使用8个字节的存储空间。

默认情况下，MySQL以一种可排序的，无歧义的格式显示DATETIME值。例如‘2008-01-01 22:11:11’，这是ANSI标准定义的日期和时间表示方法。

TIMESTAMP

保存从1970.1.1以来的秒数，它和UNIX时间戳相同。TIMESTAMP只使用4个字节的存储空间，因此它的范围比DATETIME小得多，只能表示从1970年到2038年。

如果在多个时区存储或访问数据，TIMESTAMP和DATETIME的行为将不一样。前者提供的值与市区有关系，后者则保留文本表示的日期和时间。

