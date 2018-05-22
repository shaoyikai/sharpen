在windows环境下，通过putty工具可以很方便的管理linux主机。putty运行linux命令当然没问题，可是能不能使用linux的图形界面程序呢？比如gedit编辑器。答案是肯定的！不过这要借助x11工具，如Xming。具体操作如下：

### 下载并安装Xming客户端

### 下载并安装putty

### 配置putty

选中左边菜单Session，在右边的Host Name处输入：远程主机IP， Port处：输入端口（默认22），Connection type选择SSH。

选中左边菜单的Connection->SSH->x11，勾上Enable X11 forwarding。（注意，如果你要使用图形界面，这个选项一定要勾上，否则可以省略）

### 启动Xming（是Xming而不是XLaunch命令）。注意，要使用图形界面的话，必须先启动Xming，否则可以省去。

### 启动Putty，按open按钮来连接远程服务器。

下面我们在putty里边输入图形界面命令了！
比如我需要使用 `gedit` 命令来编辑 `a.txt` 这个文本文件！


> 参考：[putty+xming远程登录Ubuntu16.04图形界面](https://www.cnblogs.com/xuanxufeng/p/6243244.html)

