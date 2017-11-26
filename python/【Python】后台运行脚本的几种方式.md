# 【Python】后台运行脚本的几种方式
#人工智能/python
我们通过 ssh 远程终端连接上Linux服务器后：

# 1 . 脚本后面加上 & 符号；(退出终端后也就终止了)
比如执行： `python test.py & `

这样test.py 这个Python脚本就会在Linux的后台执行，这样我们就可以在终端继续工作了。但是有一个问题就是，如果我们退出了终端，那么这个会话结束了，这个脚本也就运行结束了。

# 2 . nohup 命令：
为了退出终端后，命令仍然在后台运行，可以使用 nohup 命令， 比如：
```shell
$nohup sh test.sh & 

$nohup python python.py & 
```
nohup 命令运行由 Command 参数和任何相关的 Arg 参数指定的命令，忽略所有挂断（SIGHUP）信号。在注销后使用 nohup 命令运行后台中的程序。要运行后台中的 nohup 命令，添加 & （ 表示”and”的符号）到命令的尾部。

如果不将 nohup 命令的输出重定向，输出将附加到当前目录的 nohup.out 文件中。如果当前目录的 nohup.out 文件不可写，输出重定向到 $HOME/nohup.out 文件中。如果没有文件能创建或打开以用于追加，那么 Command 参数指定的命令不可调用。如果标准错误是一个终端，那么把指定的命令写给标准错误的所有输出作为标准输出重定向到相同的文件描述符。 

注意：
当断开终端，重新连接上时，进程还在运行，但是Python运行看不到任何输出，这是因为python的输出有缓冲，导致python.log3并不能够马上看到输出。
使用-u参数，使得python不启用缓冲。
所以改正命令，就可以正常使用了
nohup python -u test.py >> out.log &

# 3 . 通过 tmux 实现
这种方式依赖于tmux 的特性。我们先创建一个会话，然后在会话中运行脚本时加上 & 以后台运行的方式运行， 即使我们退出了终端， tmux中session依然在运行。

# 4 . 实现运行失败后自动重启的特性
比如我们要运行 test.py  后台永久运行，并失败后自动重启：

我们先写好 test.py ，然后写一个shell 脚本：
```shell
while true
do
    python test.py &
done
```
然后在 tmux 的 session中运行 或则是通过 nohup 命令运行。






