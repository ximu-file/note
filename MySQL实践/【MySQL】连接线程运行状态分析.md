# MySQL连接线程运行状态



我们可以使用  `show full processlist `  命令来获取当前MySQL的所有连接线程运行状态。\



以下是一个实例：

```mysql
mysql> show full processlist;
+-------+------+----------------------+-------+---------+------+-------+-----------------------+
| Id    | User | Host                 | db    | Command | Time | State | Info                  |
+-------+------+----------------------+-------+---------+------+-------+-----------------------+
|   500 | root | 123.206.13.151:59907 | zhihu | Sleep   |  165 |       | NULL                  |
|   501 | root | 123.206.13.151:59908 | zhihu | Sleep   |    3 |       | NULL                  |
|   502 | root | 123.206.13.151:59909 | zhihu | Sleep   |  171 |       | NULL                  |
|   503 | root | 123.206.13.151:59910 | zhihu | Sleep   |  176 |       | NULL                  |
|   504 | root | 123.206.13.151:59912 | zhihu | Sleep   |  150 |       | NULL                  |
|   505 | root | 123.206.13.151:59913 | zhihu | Sleep   |  167 |       | NULL                  |
|   506 | root | 123.206.13.151:59914 | zhihu | Sleep   |  161 |       | NULL                  |
|   507 | root | 123.206.13.151:59915 | zhihu | Sleep   |  160 |       | NULL                  |
|   508 | root | 123.206.13.151:59916 | zhihu | Sleep   |  155 |       | NULL                  |
|   509 | root | 123.206.13.151:59917 | zhihu | Sleep   |  154 |       | NULL                  |
|   510 | root | 123.206.13.151:59918 | zhihu | Sleep   |  142 |       | NULL                  |
|.....................略............

|   590 | root | 123.206.13.151:59999 | zhihu | Sleep   |  105 |       | NULL                  |
|   591 | root | 123.206.13.151:60000 | zhihu | Sleep   |   91 |       | NULL                  |
|   592 | root | 123.206.13.151:60001 | zhihu | Sleep   |   76 |       | NULL                  |
|   593 | root | 123.206.13.151:60002 | zhihu | Sleep   |   67 |       | NULL                  |
|   594 | root | 123.206.13.151:60003 | zhihu | Sleep   |   55 |       | NULL                  |
|   595 | root | 123.206.13.151:60005 | zhihu | Sleep   |   22 |       | NULL                  |
|   596 | root | 123.206.13.151:60006 | zhihu | Sleep   |   34 |       | NULL                  |
|   597 | root | 123.206.13.151:60007 | zhihu | Sleep   |   30 |       | NULL                  |
|   598 | root | 123.206.13.151:60008 | zhihu | Sleep   |   20 |       | NULL                  |
|   599 | root | 123.206.13.151:60009 | zhihu | Sleep   |  182 |       | NULL                  |
|   600 | root | 123.206.13.151:60010 | zhihu | Sleep   |  146 |       | NULL                  |
|   601 | root | 123.206.13.151:60011 | zhihu | Sleep   |   99 |       | NULL                  |
| 19280 | root | localhost            | zhihu | Query   |    0 | NULL  | show full processlist |
+-------+------+----------------------+-------+---------+------+-------+-----------------------+
102 rows in set (0.00 sec)
```



上面的 Command 列就是显示了当前线程的状态。、



一些常见的状态变化如下：



|              状态               |                    含义                    |
| :---------------------------: | :--------------------------------------: |
|             Sleep             |             线程正在等待客户端发送新的请求              |
|             Query             |          线程正在执行查询或则正在将结果集返回客户端           |
|            Locked             |           MySQL服务器层，该线程正在等待表锁。           |
|   Analyzing and statistics    |        线程正在收集存储引擎统计的信息并生成查询的执行计划         |
| Copying to tmp table[on disk] | 线程正在执行查询并将其结果集复制到一个临时表中，一般这种状态出现在group by、文件排序或则Union操作。 on disk 标记表示MySQL正在将内存临时表放在磁盘上。 |
|        Sorting result         |               线程正在对结果集进行排序               |
|         Sending data          |    线程可能在多个状态之间传送数据或则生成结果集或则在向客户端返回数据。    |




