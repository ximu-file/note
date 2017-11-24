# 【Linux】系统管理常用命令


# 1 . 查看系统当前登录用户命令

```
whoami   # 显示当前登录用户
who      # 显示所有的用户
last     # 显示用户登录记录
w        # 显示用户使用记录
```

# 2 . ubuntu下给系统增加root用户

方法：在终端中输入：#sudo passwd root
之后要求你输入两次root用户的密码，重启后就可以登陆root用户了。
退出root权限方法：#exit
若想禁用 root 帐号：# sudo passwd  -l  root

# 3 . ssh root用户报错Permission denied, please try again
博客地址： http://blog.csdn.net/u010853261/article/details/54811554

解决办法： 
(1)要修改root的ssh权限，即修改 _etc_ssh/sshd_config文件中 
```
PermitRootLogin without-password 
改为 PermitRootLogin yes 
```

OK了，注意这时一定要重启服务器，然后才能生效。重启之后就可以用root用户登录了。

# 4 .在文件后面添加行
场景：将 alias ll=‘ls -al’ 添加到配置文件.bash_profile的最后一行；
`echo  “alias ll=‘ls -al’” >> ~/.bash_profile`

# 5 . Linux 系统重启命令

```
shutdown -r now  # 表示现在重启计算机！
reboot           # 也表示重启！
```

# 6 . Ubuntu 更改主机名

```
# 1.修改 /etc/hosts 文件： 
gedit  /etc/hosts

# 2.修改 /etc/hostname 文件： 
gedit  /etc/hostname 

# 3. 然后关机重启

# 4. 开机后查看主机名： 
hostname
# 或则
uname -n

# 5.查看系统版本：
uname -a
```

# 7 . 获取Linux的系统磁盘使用情况(余量)
`df -hl`

# 8 . 查看文件的大小

 du命令用于显示目录或文件的大小。

-h或--human-readable 以K，M，G为单位，提高信息的可读性。
-s或--summarize 仅显示总计。

# 9 . 解压文件

```
# （1）tar.gz 解压
tar  xzf  文件

# 解压到指定的目录， v参数表示显示解压的过程， -C表示指定目标目录
tar -xvzf  压缩文件 -C DirPath

# （2）.tgz
解压：tar zxvf FileName.tgz
压缩：tar zcvf FileName.tgz FileName
```

# 10 . 查看端口号监听情况
```
（1）netstat -anl | grep 80    // 查看80端口使用情况

（2）netstat -an | grep 3306  //查看3306端口使用情况

（2）netstat -ntlp            //查看当前所有tcp端口

（3）netstat -ntulp |grep 80  //查看所有80端口使用情况
```

# 11 . 查看系统是否安装了mysql
```
（1）netstat  -tap |  grep  mysql

（2）查看mysql的版本:  mysql -V
```


# 12 . Linux 文件内容查看
Linux系统中使用以下命令来查看文件的内容：
```
cat  由第一行开始显示文件内容
tac  从最后一行开始显示，可以看出 tac 是 cat 的倒著写！
nl   显示的时候，顺道输出行号！
more 一页一页的显示文件内容
less 与 more 类似，但是比 more 更好的是，他可以往前翻页！
head 只看头几行
tail 只看尾巴几行

查看文件内容--cat、vim、head、tail、more

（1）显示时同时显示行号:
$cat -n

（2）按页显示列表内容:
$ls -al | more

（3）只看前10行:
$head - 10 **

（4）显示文件第一行:
$head -1 filename

（5）显示文件倒数第五行:
$tail -5 filename

（6）动态显示文本最新信息:
$tail -f crawler.log
```

# 13 . 使用命令帮助
`whatis  which  whereis`

```
（1）查看命令的简要说明--whatis
简要说明命令的作用（显示命令所处的man分类页面）:
$whatis command

（2）查看命令的路径--which
查看程序的binary文件所在路径:
$which command

（3）查看程序的搜索路径--whereis
$whereis command
当系统中安装了同一软件的多个版本时，不确定使用的是哪个版本时，这个命令就能派上用场；
```

# 14 . 文件及目录管理
（1）查找目录及文件--find/locate

查找目标文件夹中是否有obj文件:

$find ./ -name '*.obj'

# 15.清理内存

```
# 1.清理前内存使用情况 
free -m

# 2.开始清理命令：
echo 1 > /proc/sys/vm/drop_caches

# 3.清理后内存使用情况 
free -m

# 4.完成!
```

# 16.查看某个端口被哪个程序所占用
```
sudo lsof -i :端口号
```

# 17.杀死进程

`sudo kill -9 PID`



# 18. Linux性能监控命令

命令集合：
`top  free  vmstat  sar`

1 . 监控CPU：sar

2 . 查询内存：free

3 . Linux下任务管理器：top

4 . 监视内存使用情况：vmstat




























