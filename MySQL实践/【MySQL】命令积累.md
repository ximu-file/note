# MySQL常用命令



## 登录

1. 登录本地终端：
  `mysql -uroot -pvstar123`
2. 远程登录服务端MySQL
  `mysql -h localhost -uroot -pvstar123`



## 日志

1. Linux下清理日志文件

[mysql日志清理 - osxlinux - 博客园](http://www.cnblogs.com/osxlinux/p/3865721.html)



查看日志相关的配置变量

`show variables like '%log%'`

```
+-----------------------------------------+-----------------------------------+
| Variable_name                           | Value                             |
+-----------------------------------------+-----------------------------------+
| back_log                                | 50                                |
| binlog_cache_size                       | 32768                             |
| binlog_direct_non_transactional_updates | OFF                               |
| binlog_format                           | STATEMENT                         |
| binlog_stmt_cache_size                  | 32768                             |
| expire_logs_days                        | 10                                |
| general_log                             | OFF                               |
| general_log_file                        | /var/lib/mysql/louyuting.log      |
| innodb_flush_log_at_trx_commit          | 1                                 |
| innodb_locks_unsafe_for_binlog          | OFF                               |
| innodb_log_buffer_size                  | 8388608                           |
| innodb_log_file_size                    | 5242880                           |
| innodb_log_files_in_group               | 2                                 |
| innodb_log_group_home_dir               | ./                                |
| innodb_mirrored_log_groups              | 1                                 |
| log                                     | OFF                               |
| log_bin                                 | OFF                               |
| log_bin_trust_function_creators         | OFF                               |
| log_error                               | /var/log/mysql/error.log          |
| log_output                              | FILE                              |
| log_queries_not_using_indexes           | OFF                               |
| log_slave_updates                       | OFF                               |
| log_slow_queries                        | OFF                               |
| log_warnings                            | 1                                 |
| max_binlog_cache_size                   | 18446744073709547520              |
| max_binlog_size                         | 104857600                         |
| max_binlog_stmt_cache_size              | 18446744073709547520              |
| max_relay_log_size                      | 0                                 |
| relay_log                               |                                   |
| relay_log_index                         |                                   |
| relay_log_info_file                     | relay-log.info                    |
| relay_log_purge                         | ON                                |
| relay_log_recovery                      | OFF                               |
| relay_log_space_limit                   | 0                                 |
| slow_query_log                          | OFF                               |
| slow_query_log_file                     | /var/lib/mysql/louyuting-slow.log |
| sql_log_bin                             | ON                                |
| sql_log_off                             | OFF                               |
| sync_binlog                             | 0                                 |
| sync_relay_log                          | 0                                 |
| sync_relay_log_info                     | 0                                 |
+-----------------------------------------+-----------------------------------+
```





## 进程

1. 该命令可以查看到mysql所的进程状态，所以对show processlist 的状态进行理解和确认，也就显得非常必要了。
  `show processlist` 命令



## 表相关信息

1. 显示表的相关信息

`show table status like 'user'`



## 慢查询

查询慢查询相关的变量内容
`show variables like "%slow%";`

```
mysql> show variables like "%slow%";
+---------------------+-----------------------------------+
| Variable_name       | Value                             |
+---------------------+-----------------------------------+
| log_slow_queries    | OFF                               |
| slow_launch_time    | 2                                 |
| slow_query_log      | OFF                               |
| slow_query_log_file | /var/lib/mysql/louyuting-slow.log |
+---------------------+-----------------------------------+
```

