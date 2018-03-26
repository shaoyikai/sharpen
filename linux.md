加入安装了图形界面，如何切换命令行与GUI呢？

- `ctrl + alt + f1` 进入命令行模式
- `startx` 启动GUI
- `exit` 注销当前用户

指令太长的时候，可以用输入`\` + `Enter`（紧接着输入回车）的方式，使指令连续到下一行。

安全关机：
- `shutdown -h now`
- `shutdown -h 20:00`
- `shutdown -h +10`
- `shutdown -r now`
- `shutdown -r +30 'The system will restart after 30 mins.'`
- `sync;sync;sync; reboot`

- `poweroff` 切断电源

- `systemctl reboot`
- `systemctl poweroff`


### 权限

改变群组

- `chgrp staff /u` change the group of /u to 'staff'
- `chgrp -hR staff /u` change the group of /u and subfiles to 'staff'

改变拥有者

- `chown yikai a.txt`
- `chown yikai dir/`
- `chown yikai:yikai dir/`

改变文件的权限

- `chmod 755 a.txt`
- `chmod -R 755 dir/`

牢记`rwx`对应`421`即可
> 
	r - 4
	w - 2
	x - 1 (如果是file代表是否可以执行，如果是directory代表是否可以进入此目录)
	7: rwx
	6: rw-
	5: r-x
	4: r--
	3: -wx
	2: -w-
	1: --x
	0: ---

举例：
-rwxr-xr-x 对应 755

修改权限的另一种方式：
>
	- u - user
	- g - group
	- o - others
	- a - all

举例：
-rwxr-xr-x 对应语法为 `chmod u=rwx,go=rx a.txt`
-rwxr-xr-- 对应语法为 `chmod u=rwx,g=rx,o=r a.txt`
给所有人添加可写权限 `chmod a+w a.txt`
给所有人去掉可执行权限 `chmod a-x a.txt`


usr = "unix software resource"

### 文件管理

- `cd`: 切换目录
- `pwd`: 显示当前目录，`pwd -P`: 显示真实目录
- `mkdir`：创建新目录，
- `mkdir -P test1/test2/test3/test4`：递归创建，
- `mkdir -m 711 test`：创建同时设置权限
- `rmdir`：删除一个空的目录
- `rmdir -p test1/test2/test3/test4`：连同“上层”“空的”目录一并删除
- `rm -r test` 删除test下所有的内容

- `ls` 列出目录内容
- `cp` 复制
- `mv` 移动
- `rm` 删除

- `cat` 从第一行开始显示内容
- `tac` 从最后一行开始显示
- `nl` 显示的时候，顺道输出行号
- `more` 一页一页显示内容
- `lese` 类似more，但是可以往前翻页
- `head` 只看头几行
- `tail` 只看尾巴几行
- `od` 以二级制方式读取文件内容，`echo abc|od -t oCc` 获取abc对应的ascii码
- `head -n 20 /etc/man_db.conf | tail -n 10` 显示第11到第20行内容 
- `touch [-acmt] filename` touch 命令可以修改文件ctime|mtime|atime，或创建空文件


### 修改环境变量
`PATH="${PATH}:/root"` 将/root添加到PATH变量

### 软件管理

### 网络


