# 【tmux】1.tmux命令


# 1. tmux的配置文件location
 ~/.tmux.conf

# 2. tmux前缀快捷键的修改
为了使自身的快捷键和其他软件的快捷键互不干扰，Tmux 提供了一个快捷键前缀。当想要使用快捷键时，需要先按下快捷键前缀，然后再按下快捷键。Tmux 所使用的快捷键前缀默认是组合键 Ctrl-b（同时按下 Ctrl 键和 b 键）。若要将快捷键前缀变更为 Ctrl-a ，请将以下配置加入到 Tmux 的配置文件 ~/.tmux.conf 中：

```shell
unbind C-b
set -g prefix C-a
```
更改之后必须重新启动终端才会生效。

假如你想通过快捷键列出当前 Tmux 中的所有会话（对应的快捷键是 s），那么你只需要做以下几步：
* 按下组合键 Ctrl-b (Tmux 快捷键前缀)
* 放开组合键 Ctrl-b
* 按下 s 键

# 3. tmux 会话的快捷键
### 1）新建会话
`tmux new -s session-name`

### 2）显示所有会话：
```
# tmux会话模式下：
Ctrl-b s
# 终端模式下：
tmux ls
```

### 3）接入一个之前的会话
```  
# 接入第一个可用的会话    
tmux a

# 通过参数指定一个想接入的会话：
tmux a -t session-name
```

### 4）从会话中断开
```   
# 可以使用 detach 命令断开已有的会话（因此才会有稍后重新接入会话这么一说）。
tmux detach

# 也可以使用快捷键断开会话：
Ctrl-b d
```

### 5）关闭会话

```
# 要关闭会话的话，可以使用如下的命令，该命令和接入会话时所使用的命令很像：
$ tmux kill-session -t session-name
```

### 6）重命名会话名称
```
# 重命名 abc 会话名称为 cba
tmux rename -t abc cba
```

### 7）在 tmux 模式下运行 tmux 命令：
	* 首先输入ctrl+b，然后在终端底部可以输入新的命令。
	* 比如我想在tmux下新建一个 session，可以做法如下：
```
按下ctrl+a 然后输入":" 再输入 new -s jiang

这样就新建一个 jiang 的session会话
```

# 4. tmux 窗口管理的快捷键
在tmux模式下：

```
Ctrl B  d       #隐藏会话

Ctrl B  c       #新开一个窗口

Ctrl B   &      #退出当前窗口

Ctrl B  ，       #重命名窗口

Ctrl B   数字    #切换到第几个窗口

Ctrl B   n       #切换到下个窗口

Ctrl B  p        #切换到上个窗口

Ctrl B   l       #切换到最后一个窗口

Ctrl B    w      #以菜单的方式显示和选择窗口

Ctrl B  ”        #横向分割窗口

Ctrl B  %        #纵向分割窗口

Ctrl B   o       #跳到下一个分割窗口

Ctrl B   上下左右  #跳到指定方向的分割窗口

Ctrl B    x       #关闭当前分割窗口

Ctrl B   ！       #关闭所有分割窗口

C Ctrl-方向键      #调整分割窗口大小

Ctrl B   ？       #显示快捷键帮助

Ctrl B   t        #显示时钟

Ctrl B  [         #进入拷贝模式，可以使用上下左右键或者触摸板来翻页，Ctrl + C 退出

Ctrl B  ]         #粘贴

Ctrl B space      #开始复制（拷贝模式）
```

