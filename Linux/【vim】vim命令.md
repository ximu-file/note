# 【vim】2.vim命令
#技术积累/linux
下面是百度的vim 配置文件， 使用时记得改成  .vimrc，放在用户目录下。

参考文档：

http://www.cnblogs.com/softwaretesting/archive/2011/07/12/2104435.html

# 1 . 查找命令
/text     查找text，向下查找，按n健查找下一个，按N健查找前一个。
?text    查找text，反向查找，按n健查找下一个，按N健查找前一个。

## 跳转到某一行
vim命令行模式下：
`ngg`   n表示行数
或则
`:123` 然后回车

less命令跳转到某一行
`:888g` 自动跳转

# 2 . 编辑命令
u   撤销（Undo）
U   撤销对整行的操作
Ctrl + r   重做（Redo），即撤销的撤销。


Ctrl + e 向下滚动一行
Ctrl + y 向上滚动一行
Ctrl + d 向下滚动半屏
Ctrl + u 向上滚动半屏
Ctrl + f 向下滚动一屏
Ctrl + b 向上滚动一屏

dd      删除当前行
dj       删除上一行
dk      删除下一行
10dd  删除当前行开始的10行。

yy 拷贝当前行
nyy 拷贝当前后开始的n行，比如2yy拷贝当前行及其下一行。
p  在当前光标后粘贴,如果之前使用了yy命令来复制一行，那么就在当前行的下一行粘贴。


# 非编辑命令
1）设置显示行号：
`set nu`








