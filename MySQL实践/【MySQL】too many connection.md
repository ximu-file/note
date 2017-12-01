# 【MySQL】too many connection

使用MySQL时候经常碰到这个问题，表示MySQL收到的连接请求太高了。原因有两种：
（1）一种是访问量确实很高，MySQL服务器抗不住，这个时候就要考虑增加从服务器分散读压力。
（2）另外一种情况是MySQL配置文件中max_connections值过小。
（3）还有一种低级错误可能性是，connect了MySQL之后，忘记了在finally里面close.

## （1）MySQL服务器最大连接数

`show variables like 'max_connections';`

## （2）查询一下当前服务器响应的最大连接数：

`show global status like 'max_used_connections';`

如果max_used_connections远远小于max_connections，说明当前服务器连接有较多空闲，如果Max_used_connections非常接近max_connections，甚至等于max_connections，那就需要加大max_connections值了。



# 解决方案

## 方案一：修改/etc/mysql/my.cnf 文件里面配置

更改此文件的： `max_connections=N` 来更改最大的连接数，但是这种更改必须重启MySQL服务器才会生效，但是我们的生产服务器是不能随便停的。

## 方案二: 热更改

执行命令：
`gdb -p $(cat /var/run/mysqld/mysqld.pid) -ex "set max_connections=500" -batch`

上面的 `/var/run/mysqld/mysqld.pid` 指当前mysql服务的pid文件，根据自己的存放指定即可，后面是修改mysqld内存中max_connections为多少。

我们可以这样查看自己的pid文件存放在那里：
`mysql> show variables like '%pid%';`
输出：
```
+---------------+----------------------------+
| Variable_name | Value                      |
+---------------+----------------------------+
| pid_file      | /var/run/mysqld/mysqld.pid |
+---------------+----------------------------+
1 row in set (0.00 sec)
```







