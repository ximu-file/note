# 【Linux】wget、curl、scp


# 1 . wget 命令：主要用于下载文件
下面就给出一些常用的命令实例：
```
# 1）下载文件：
$ wget  http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.9/zookeeper-3.4.9.tar.gz

# 支持断点下载：-c 参数
$ wget  -c  http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.9/zookeeper-3.4.9.tar.gz

# 设置文件名： -O wordpress.zip
$wget -O zookeeper.tar.gz  http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.9/zookeeper-3.4.9.tar.gz
```


# 2 . curl 命令：主要用于调试API，也可以用来下载文件
### 1）以下以测试 REST 资源的接口为例：
`curl -i -H "Accept: application/json" -X POST -d "firstName=james" http://119.23.23.203/vr/open`
参数如下：
-o  filename        ：将文件保存为命令行中指定的文件名的文件中
-O                        ：使用URL中默认的文件名保存文件到本地
 i                            – 显示响应头
H/—header          – 向资源请求头部 header
X                           – 传递HTTP名称，也就是GET POST PATCH DELETE等等
d                            – 传入双引号中的表单参数，多个参数用’&’分割

### 2）下面还是给出一个重要的参数含义列表：

-A/--user-agent    设置用户代理发送给服务器

-b/--cookie           cookie字符串或文件读取位置

--data-binary        以二进制的方式post数据, payload 里面传入son字符串时，设置这个

-e/--referer           来源网址

-I/--head              只显示请求头信息

-i/--include           response输出时包括protocol头信息

### 3） 再给出一个实例：

`curl 'http://123.206.13.126:8580/v3/users/b46290268d9f45648d69320d9d5b8cb1' -H 'X-Auth-Token: 0e5d1b77a57a4eee8bbc6ac3a7c76a37' -H 'Content-Type: application/json'  --data-binary '{"user":{"name":"proxy"}}'`


# 3 . scp 命令
这个命令主要用来在Linux系统之间传文件：

### 1）从本地复制到远端服务器
复制文件
$scp local_folder remote_username@remote_ip:remote_folder

递归复制目录文件：
$scp -r local_folder remote_username@remote_ip:remote_folder

### 2）从服务端下载到本地：命令一样，只是把scp 后面的目录位置交换就行

$scp -r root@120.25.12.125:_home_root_others_ _home_space_music_

