# MySQL索引

核心内容：

## 索引基础

1.  B树索引的原理
2.  哈希索引的原理
3.  索引匹配有效的场景



## 索引的优点

1.  大大减少了服务器需要扫描的数据量
2.  可以帮助服务器避免排序和临时表
3.  可以将随机IO转换成顺序IO



# 高性能索引策略

基于具体的使用场景正确的创建索引是g高性能查询的基础，下面将介绍各种索引以及其适用场景。

> 1. 前缀索引和索引的选择性
> 2. 多列索引以及选择合适的多列索引顺序
> 3. 聚簇索引
> 4. 覆盖索引



##1. 前缀索引和索引的选择性

**前缀索引**：有时候我们添加索引的字段可能是一个很长的字符串，这时候索引非常大而且变慢，所以我们可以设置索引字段的开始的部分字符串，这样可以大大节省索引空间，从而提升索引的效率，但是也会降低索引的选择性。



**索引的选择性**：不重复的索引值(索引字段)和数据表中的记录总数的比值。范围从无穷小到1之间，索引的选择性越高，性能越高（其实也就是能够过滤更多的行）。



比如：有如下一张表：

```
+------------+--------------+------+-----+---------+----------------+
| Field      | Type         | Null | Key | Default | Extra          |
+------------+--------------+------+-----+---------+----------------+
| id         | int(11)      | NO   | PRI | NULL    | auto_increment |
| user_token | varchar(100) | YES  | MUL | NULL    |                |
| location   | varchar(255) | YES  |     | NULL    |                |
| business   | varchar(255) | YES  |     | NULL    |                |
| sex        | varchar(255) | YES  |     | NULL    |                |
| employment | varchar(255) | YES  |     | NULL    |                |
| education  | varchar(255) | YES  |     | NULL    |                |
| username   | varchar(255) | YES  |     | NULL    |                |
| url        | varchar(255) | YES  |     | NULL    |                |
| agrees     | int(11)      | YES  |     | NULL    |                |
| thanks     | int(11)      | YES  |     | NULL    |                |
| asks       | int(11)      | YES  |     | NULL    |                |
| answers    | int(11)      | YES  |     | NULL    |                |
| posts      | int(11)      | YES  |     | NULL    |                |
| followees  | int(11)      | YES  |     | NULL    |                |
| followers  | int(11)      | YES  |     | NULL    |                |
| hashId     | varchar(255) | YES  |     | NULL    |                |
+------------+--------------+------+-----+---------+----------------+
```



表中的索引如下：

```
+-------+------------+----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table | Non_unique | Key_name       | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------+------------+----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| user  |          0 | PRIMARY        |            1 | id          | A         |     1395477 |     NULL | NULL   |      | BTREE      |         |               |
| user  |          1 | id             |            1 | id          | A         |     1395477 |     NULL | NULL   |      | BTREE      |         |               |
| user  |          1 | key_user_token |            1 | user_token  | A         |     1395477 |     NULL | NULL   | YES  | BTREE      |         |               |
+-------+------------+----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
```



对于索引列 user_token 我们先来看看数据分布：

表中的总记录数是：

```
+----------+
| count(*) |
+----------+
|  1370672 |
+----------+
```



当我们以user_token的前缀5位作为分组条件进行排序时，执行结果如下，

```
mysql> select count(*) as cnt, left(user_token, 5) as pref from user group by pref order by cnt desc limit 10;
+-------+-------+
| cnt   | pref  |
+-------+-------+
| 45152 | wang- |
| 42363 | zhang |
| 32110 | Chen- |
| 26755 | xiao- |
| 18230 | yang- |
| 12684 | huang |
| 11807 | zhou- |
| 11569 | zhao- |
|  8787 | jiang |
|  6928 | feng- |
+-------+-------+
10 rows in set (1.38 sec)
```



当我们以user_token的前缀10位作为分组条件进行排序时，执行结果如下，

```
mysql> select count(*) as cnt, left(user_token, 10) as pref from user group by pref order by cnt desc limit 10;
+------+------------+
| cnt  | pref       |
+------+------------+
| 3149 | wang-xiao- |
| 2727 | zhang-xiao |
| 1990 | xiao-xiao- |
| 1795 | chen-xiao- |
| 1129 | yang-yang- |
| 1058 | yang-xiao- |
| 1050 | chen-chen- |
|  807 | huang-xiao |
|  758 | miao-miao- |
|  756 | zhang-wei- |
+------+------------+
10 rows in set (21.08 sec)
```



虽然索引的选择性变高了，但是查询速度明显下降的非常厉害, 并且对于order by 或则 group by 并没有使用上索引。



**总结：**

前缀索引的优点很明显：能够使索引更小，速度更快。

但是其缺点也是很明显的：MySQL无法使用前缀索引做order by 和 group by，也无法使用前缀索引做覆盖扫描。



## 2. 多列索引以及选择合适的多列索引顺序

**多列索引**（Multiple-Column Indexes）也称为复合索引（composite index），也即同时对多个列建立索引。



比如对于一个(a, b, c) 这样一个索引，当某个查询条件有(a, b, c) 或则(a, b) 或则(a) 时，才能够利用上索引，否则就不会命中索引。那么当我们设计索引时应该考虑的点有哪些呢？

（1）正确的顺序，能够让查询命中索引；

（2）考虑更好的满足order by 和 group by 语句。



基于经验法则，一般让选择性高的索引列放在最前面，因为可以过滤更多的行。但是也要结合具体的场景，比如某个索引列的选择性最高，但是其中某条记录中该字段重复性非常高，几乎占了记录总数全部，这时候就要实际出发从代码层面和数据库层面综合考虑设计索引。



## 3. 聚簇索引

> 定义：聚簇索引并不是一种单独的索引类型，而是一种数据存储方式。InnoDB的聚簇索引实际上是在同一个结构中保存了B-Tree索引和具体的数据行。



当表中有聚簇索引的时候，它的数据行实际上存放在索引的叶子页中，术语"聚簇"表示数据行和相邻的键值紧紧存储在一起。 很明显数据行只会有一份，也就是说数据表只能有一个聚簇索引。下图展示了一个聚簇索引的数据分布图：



![330E9F34-B67A-465D-95CC-9DCEEFA8B342](../../Library/Containers/com.tencent.qq/Data/Library/Application Support/QQ/Users/1849491904/QQ/Temp.db/330E9F34-B67A-465D-95CC-9DCEEFA8B342.png)



上图中，叶子页包含了数据行的全部数据，但是节点页只包含了索引列。对于InnoDB这个存储引擎，默认选择主键聚集数据，也就是上图中的索引列就是主键。



## 4. 覆盖索引

> 定义：如果一个索引包含（覆盖）所有需要查询的字段的值(也就意味着不需要回表查询，高性能)，我们就称之为覆盖索引。



## 5. 使用索引扫描来做排序

MySQL有两种方式生成有序的结果：（1）通过排序 order by 操作，但是这种排序是文件排序，很明显是性能低的。（2）按索引顺序扫描，借助索引的有序性。



我们使用explain分析查询时候，如果type列显示是index，就表示MySQL使用了索引扫描来做排序。按照索引顺序扫描本身是非常快的，索引的有序性使得排序只需要从一条索引移动到下一条索引记录。但是，如果索引列不能满足查询的全部列，那么每次索引都需要回表查询一次记录，而这次回表又是随机IO，所以索引顺序查询往往还比顺序全表扫描慢。



当然，如果某个查询既满足索引查找排序，索引列也满足查询列要求，这样是最好的。



## 6.















# 总结

我们在设计索引和利用索引查询的时候应该时刻记住以下几点：

1. 单行访问是很慢的，因为一次单行访问一般都是随机IO，也就是说读取一个数据块只是为了获取一行数据，就会浪费很多工作。
2. 按顺序访问范围数据是很快的，原因如下：顺序IO不需要多次在磁盘寻道，比随机IO快很多。服务器如果能够按照需要的顺序读取数据就不需要额外的排序操作，并且group by 也不在需要再做排序将行按组进行聚合计算了。
3. 覆盖索引速度是很快的，根本原因在于不需要回表查询。









