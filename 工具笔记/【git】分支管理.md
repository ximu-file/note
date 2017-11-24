# 【git】分支管理


# 分支管理 git branch以及 git merge
```
（1）输出本地所有分支
git branch -r

（2）输出当前本地所有分支，以及远程所有分支
git branch -a 

（3）合并分支，可以使用git merge命令或者git rebase命令，合并指定分支到当前分支。比如下面的命令表示在当前所在分支上，合并origin/master分支到当前分支。
git merge origin/master “备注” 

（4）删除分支
git branch -d <name>

（5）切换到上个分支
git checkout -

（6） 创建分支，默认从master分支copy过来一个新的分支
# 这里如果不加后面的branchName就会列出所有的分支。
git branch branchName 

（7）切换分支，更新HEAD以指向branchname分支，以及用branchname  指向的树更新暂存区和工作区。
git checkout branchname

（8）创建一个分支并切换到新分支，也就是上面两个命令的和:先创建一个分支 branchName，然后切换到这个分支。
git checkout -b [branchName]
上面这个命令等于： git branch branchName   &&   git checkout branchName

（9）取**远程分支**并分化一个新分支mybranch
git checkout -b mybranch origin/mybranch

（10）汇总显示工作区、暂存区与HEAD的差异。下面两个命令功能一样
git checkout
git checkout HEAD

（11）用暂存区中filename文件来覆盖工作区中的filename文件。相当于撤销自上次执行git add filename以来（如果执行过）的本地filename文件的修改。
git checkout -- filename

（12）维持HEAD的指向不变。将branch所指向的提交中filename替换暂存区和工作区中相应的文件。注意会将暂存区和工作区中的filename文件直接覆盖。
git checkout branch -- filename

（13）下面命令中注意git checkout 命令后的参数为一个点（“.”）。这条命令最危险！会取消所有本地的修改（相对于暂存区）。相当于用暂存区的所有文件直接覆盖本地文件，不给用户任何确认的机会！
$ git checkout .
```
