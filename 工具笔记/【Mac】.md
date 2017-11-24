# 【Mac】
#技术积累/工具/mac

# 命令行清理内存
$ purge  

# 查看每个目录的占用硬盘大小：
-d表示深度
`sudo du -d 1 -h`

# MAC下内存分析工具
先安装
`$brew install htop`

使用：
`$htop`

# 修改launchpad的图标大小
```
# 运行“终端”程序，执行以下命令：
# 1、调整每一列显示图标数量，7 表示每一列显示7个，在我的电脑上，7个个人觉得比较不错
defaults write com.apple.dock springboard-rows -int 7
# 2、调整每一行显示图标数量，这里我用的是8
defaults write com.apple.dock springboard-columns -int 8
# 3、由于修改了每一页显示图标数量，
#defaults write com.apple.dock ResetLaunchPad -bool TRUE

# 可能需要重置Launchpad
#killall Dock
```

# Mac快捷键
command+shitf+G  finder中打开目录

# chrome 打开检查：
command + optional+i   打开network
command + shift + c 打开检查元素

# iterm2的快捷键
command+d 竖着分屏

到行首：ctrl + a
到行尾：ctrl + e

# 强制重启应用
command+option+esc  

# 刷新DNS
mac刷新DNS：
`sudo killall mDNSResponder`

linux刷新dns
`/etc/init.d/nscd restart`


# Atom快捷键
1. 打开输入命令的面板： ctrl+shift+p
2. 执行脚本：ctrl+shift+b
3. 选择编码： ctrl+shift+u

# node的运行
1. $npm run dev   表示以开发者模式启动服务器，这时通过地址http://localhost:8080/#!/login 进入项目
2. $npm run build 表示编译项目，每次更新了api.js 要记得运行这个命令。






