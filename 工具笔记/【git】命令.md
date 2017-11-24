# 【git】命令

#技术积累/工具/git                      Author-娄宇庭

推荐资料：
廖雪峰：[Git教程 - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000

所有命令:

[git-cheatsheet.pdf](../../../../../var/folders/lb/yqfbyv310tjbg1h91wvby9tr0000gn/T/net.shinyfrog.bear/BearTemp.7cWHSn/git-cheatsheet.pdf)

# 1. 配置全局变量：

## 1.1 配置全局的用户名和邮箱

```shell
git config –-global user.name “chris.lyt” 
git config –-global user.email “chris.lyt@cainiao.com”
```

## 1.2 利用生成SSHkey，并添加到git服务端(GitHub，码云等)
生成key，
`ssh-keygen -t rsa -C “1849491904@qq.com” `
然后将生成的key添加到码云中 

`ssh -T git@git.oschina.net `
上面这个命令检测是否授权成功。，若返回  Welcome to Git@OSC, yourname!  则授权成功。

# 2. 获取git仓库
## 2.1从现有项目目录或文件目录下导入所有文件到 本地Git 仓库中
`git init`
该命令就可以成功的初始化一个本地仓库。

## 2.2 从远程服务器克隆一个现有项目到本地仓库
```
# 命令的格式如下： 
git clone <版本库的网址> <本地目录名> 
# 比如：克隆远程服务器上项目到本地的相对路径purplecollar目录下。 
git clone https://git.oschina.net/louyuting/purplecollar.git purplecollar
```

# 3. 记录每次更新到仓库
文件无外乎有两种状态：已跟踪(tracked)和未跟踪(untracked)。已tracked的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已放入暂存区。 其状态如下图所示：

![1](../../../Downloads/图片/1.jpg)

## 3.1 检查当前文件状态
`git status`

## 3.2 跟踪新文件
跟踪单个文件：
```
git add <文件名> 
# 如果我们想跟踪当前目录下的所有新文件，可用如下命令 
git add .
```

`git add -A` 和 `git add .` 的区别

* `git add . `：他会监控工作区的状态树，使用它会把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。
* `git add -u` ：他仅监控已经被add的文件（即tracked file），他会将被修改的文件提交到暂存区。add -u 不会提交新文件（untracked file）。（git add --update的缩写）
* `git add -A `：是上面两个功能的合集（git add --all的缩写）

## 3.3 提交更新
`git commit -m “这里必须加上此次提交更新的备注”`

## 3.4 移除文件
要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用  `git rm filename` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。如果只是简单地从工作目录中手工删除文件，运行 git status 时就会在 “Changes not staged for commit” 部分（也就是 未暂存清单）看到。

移除文件命令： 
`git rm -f <文件名>`

# 4. 远程仓库的使用
## 4.1 git remote查看远程仓库
```
# 查看远程仓库名字：
git remote 

# 如果想查看远程仓库的地址用如下命令 
git remote -v
```
## 4.2 添加远程库等操作
```shell
# git remote show 命令加上主机名，可以查看该主机的详细信息。 
git remote show <主机名>

# git remote add 命令用于添加远程主机。 
git remote add <主机名> <网址>

# git remote rm命令用于删除远程主机。 
git remote rm <主机名>

# git remote rename命令用于远程主机的改名。 
git remote rename <原主机名> <新主机名>
```

## 4.3 git fench 命令
一旦远程主机的版本库有了更新(Git术语叫做commit)，需要将这些更新取回本地，这时就要用到git fetch命令。
```
# git fetch <远程主机名> <远程主机分支名> 
# 比如，取回origin主机的master分支。 
git fetch origin master
# 所取回的更新，在本地主机上要用”远程主机名/分支名”的形式读取。比如origin主机的master，就要用origin/master读取。 
```



## 4.5 git pull命令
git pull命令的作用是，取回远程主机所有分支的更新，再与本地的指定分支合并。它的完整格式稍稍有点复杂。
```
git pull <远程主机名> <远程分支名>:<本地分支名>`
# 实质上，这等同于先做git fetch，再做git merge。 
git fetch origin 
git merge origin/master
```

Git允许手动建立追踪关系。
`git branch –set-upstream master origin/master`
上面命令指定master分支追踪origin/master分支。

## 4.5 git push命令
git push命令用于将本地分支的更新，推送到远程主机。它的格式与git pull命令相仿。-u参数表示默认追踪的分支。
`$ git push -u <远程主机名> <本地分支名>:<远程分支名>`

# 

# 6. 更改ignore文件之后同步更新git
每次我们更新了ignore文件之后要清除缓存再commit：
```
# 重置所有缓存(注意后面有个.) 
git rm -r --cached . 

# 然后commit和push
```

# 7. UNIX环境下的自动化bash脚本提交仓库到github 
我本人一直在写一些玩具代码的时候，有提交到github上备份的习惯，由于这个是我自己写的玩具代码，也不涉及到多个分支，所以每次都是机械的敲一串相同的命令，为了提高生产力，在Mac的UNIX环境下写一个bash脚本，自动化每次提交到github，我每次只需要执行这个脚本就行了，下面给出源码：
```
#!/bin/bash

echo "开始提交代码到github"
echo "git add ."
git add .

echo "git commit"
read -p "请输入commit的注释信息:" comment
git commit -m “$comment”

echo "git fetch origin master"
git fetch origin master

echo "git merge origin/master"
git merge origin/master

echo "git push origin master:master"
git push origin master:master

sleep 5s #睡眠5s
```

这个脚本大大提交了我的生产效率。

#
# 9. 自定义git
让Git显示颜色，会让命令输出看起来更醒目：

`git config --global color.ui true`





