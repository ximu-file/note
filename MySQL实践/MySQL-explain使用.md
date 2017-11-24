# explain语句分析

explain命令是查看查询优化器如何决定执行查询的主要方法。



下面给出一个explain 语句执行结果的实例：

```
mysql> explain select count(*) as cnt, left(user_token, 5) as pref from user group by pref order by cnt desc limit 10;
+----+-------------+-------+-------+---------------+----------------+---------+------+---------+----------------------------------------------+
| id | select_type | table | type  | possible_keys | key            | key_len | ref  | rows    | Extra                                        |
+----+-------------+-------+-------+---------------+----------------+---------+------+---------+----------------------------------------------+
|  1 | SIMPLE      | user  | index | NULL          | key_user_token | 303     | NULL | 1395937 | Using index; Using temporary; Using filesort |
+----+-------------+-------+-------+---------------+----------------+---------+------+---------+----------------------------------------------+
```



分别解释一下id、select_type、table、type、possible_keys、key、key_len、ref、rows、Extra分别代表什么含义。



|      字段名      |                    含义                    |
| :-----------: | :--------------------------------------: |
|      id       | 表示一个标号、标识select所属于的行。如果语句中没有子查询或则联合，那就只会有唯一的select，此时id就唯一是1。 |
|  select_type  | 标识当前行的SELECT是简单select还是复杂select，有以下5种可能：SIMPLE(查询不包括子查询和Union，否则最外层被标记为PRIMARY，其余行被标记成后面四种中的一个)、SUBQUERY(子查询)、DERIVED(表示包含在FROM子句的子查询中的SELECT临时表)、UNION(联合查询)、UNION RESULT. |
|     table     |               标识当前行所访问的表名。               |
|     type      | MySQL如何查询表中的行的类型，依次从最差到最优如下：ALL(全表扫描) 、index(也是全表扫描，只不过是按照索引顺序扫描的，优点是避免了排序)、range(扫描范围是一个有限制的索引扫描)、ref(索引访问，返回所有匹配某个单值的行)、eq_ref(也是索引访问，但是只返回最多一条符合条件的记录，一般是主键索引或则唯一索引)、const,system(对查询某部分进行优化并转换成常量，比如主键作为where查询条件)、NULL(在SQL优化阶段能够分解查询语句，在SQL执行阶段不需要访问索引或则表，比如`explain select min(id) from user;`) |
| possible_keys |               列出查询可以使用哪些索引               |
|      key      |         显示MySQL决定使用哪个索引来优化对该表的访问         |
|    key_len    |            显示了MySQL在索引里使用的字节数            |
|      ref      |     显示了之前的表在key列记录的索引中查找值所使用的列或则常量。      |
|     rows      |         MySQL估计为了找到所需要的行而要读取的行数          |
|     Extra     | 包含不适合在其余列显示的额外信息，比如：Using index(表示使用了覆盖索引，不用回表查看)、Using where(MySQL将在存储引擎检索行后再进行过滤)、Using temporary(对查询结果排序时会使用临时表)、Using filesort(MySQL会对结果进行一个外部索引排序而不是按索引顺序从表里读取行)。 |















