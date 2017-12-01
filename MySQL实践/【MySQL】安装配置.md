# Mac下的配置
1.拷贝/usr/local/MySQL/support-files下的任意一个*.cnf文件到/etc/my.cnf；//苹果系统

2.在my.cnf文件的［client］后面添加一句default-character-set=utf8（ 不是default_character_set=utf8，这个配置会导致MySQL启动不了），关键在这里的配置，在［mysqld］后面添加如下三句：
default-storage-engine=INNODB
character-set-server=utf8
collation-server=utf8_general_ci；
3.保存退出；
4.重新启动mysql服务器就可以。

修改操作：打开/etc/my.cnf,在属性组mysqld下面添加参数如下：
[mysqld]
interactive_timeout=28800000
wait_timeout=28800000

# 修改登录权限，所有ip都可以登录
http://jingyan.baidu.com/article/0202781161be971bcc9ce51c.html

修改密码：
>use mysql;

>update user set password=password('新密码') where user='root';

>FLUSH PRIVILEGES;

重新登录就行了。
