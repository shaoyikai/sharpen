加入安装了图形界面，如何切换命令行与GUI呢？

- `ctrl + alt + f1` 进入命令行模式
- `startx` 启动GUI
- `exit` 注销当前用户

指令太长的时候，可以用输入`\` + `Enter`（紧接着输入回车）的方式，使指令连续到下一行。

安全关机
>	
	`shutdown -h now`
	`shutdown -h 20:00`
	`shutdown -h +10`
	`shutdown -r now`
	`shutdown -r +30 'The system will restart after 30 mins.'`
	`sync;sync;sync; reboot`
	`poweroff` 切断电源
	`systemctl reboot`
	`systemctl poweroff`


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

举例
> 
	-rwxr-xr-x 对应 755

修改权限的另一种方式：
>
	u - user
	g - group
	o - others
	a - all

举例
>
	-rwxr-xr-x 对应语法为 `chmod u=rwx,go=rx a.txt`
	-rwxr-xr-- 对应语法为 `chmod u=rwx,g=rx,o=r a.txt`
	给所有人添加可写权限 `chmod a+w a.txt`
	给所有人去掉可执行权限 `chmod a-x a.txt`


usr = "unix software resource"

`umask -S` 目前使用者在创建文件或目录时的权限默认值

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

当我们登录系统之后创建一个文件总是有一个默认权限的，那么这个权限是怎么来的呢？这就是umask干的事情。umask设置了用户创建文件的默认权限，它与chmod的效果刚好相反，umask设置的是权限“补码”，而chmod设置的是文件权限码。一般在/etc/profile、$ [HOME]/.bash_profile或$[HOME]/.profile中设置umask值。

系统管理员必须要为你设置一个合理的 umask值，以确保你创建的文件具有所希望的缺省权限，防止其他非同组用户对你的文件具有写权限。在已经登录之后，可以按照个人的偏好使用umask命 令来改变文件创建的缺省权限。相应的改变直到退出该shell或使用另外的umask命令之前一直有效。一般来说，umask命令是在/etc /profile文件中设置的，每个用户在登录时都会引用这个文件，所以如果希望改变所有用户的umask，可以在该文件中加入相应的条目。如果希望永久 性地设置自己的umask值，那么就把它放在自己$HOME目录下的.profile或.bash_profile文件中。

计算出你的umask值：
>
	umask值002 所对应的文件和目录创建缺省权限分别为6 6 4和7 7 5。
	umask值022 所对应的文件和目录创建缺省权限分别为6 4 4和7 5 5。

修改umask的值：
>
	`umask 002`


chattr 设置文件的隐藏属性：`chattr [+-=] [ASacdistu] filename`
>
	+ 增加某一个特殊参数，其他原本存在的则不动
	- 移除某一个特殊参数，其他原本存在的则不动
	= 设置一定，且仅有后面接的参数

- [A] 当设置了 A 这个属性时，若你有存取此文件（或目录）时，他的存取时间 atime 将不会被修改，
可避免 I/O 较慢的机器过度的存取磁盘。（目前建议使用文件系统挂载参数处理这个项目）
- [a] 当设置 a 之后，这个文件将只能增加数据，而不能删除也不能修改数据，只有root 才能设置这属性
- [i] 这个 i 可就很厉害了！他可以让一个文件“不能被删除、改名、设置链接也无法写入或新增数据！”
对于系统安全性有相当大的助益！只有 root 能设置此属性
- [s] 当文件设置了 s 属性时，如果这个文件被删除，他将会被完全的移除出这个硬盘空间，
所以如果误删了，完全无法救回来了喔！
- [u] 与 s 相反的，当使用 u 来设置文件时，如果该文件被删除了，则数据内容其实还存在磁盘中，
可以使用来救援该文件喔！

> xfs 文件系统仅支持 AadiS 而已

lsattr 显示文件隐藏属性：`lsattr [-adR] filename`
>
	[a] 将隐藏文件的属性也秀出来；
	[d] 如果接的是目录，仅列出目录本身的属性而非目录内的文件名；
	[R] 连同子目录的数据也一并列出来！

### 搜索

查找命令所在位置的方法：

- which ls
- which which
- type history （history 是shell内嵌）

which这个指令是根据“PATH”这个环境变量所规范的路径，去搜寻“可执行文件”的文件名。当which找不到时可以尝试用type查找。

文件文件名的搜索
- `whereis [-bmsu] filename`

whereis 仅搜索/bin /sbin 以及 /usr/share/man 下面的page文件，跟几个比较特殊的目录来处理而已，所以速度较快，当然有些文件会找不到。可以通过 `whereis -l` 来查询一下到底查询了多少个目录。

`find [PATH] [option] [option]` 
>
	-mtime n ：n 为数字，意义为在 n 天之前的“一天之内”被更动过内容的文件；
	-mtime +n ：列出在 n 天之前（不含 n 天本身）被更动过内容的文件文件名；
	-mtime -n ：列出在 n 天之内（含 n 天本身）被更动过内容的文件文件名。
	-newer file ：file 为一个存在的文件，列出比 file 还要新的文件文件名

例如
>
	`find / -mtime 0` 从现在开始到 24 小时前有变动过内容的文件都会被列出来！
	`find / -mtime 3` 三天前的 24 小时内有变动过的文件都被列出
	`find /var -mtime -4` 4天内被更动过的文件文件名
	`find /var -mtime 4` 4天前的那一天”
	`find / -nouser` 搜寻系统中不属于任何人的文件
	`find / -name passwd` 找出文件名为 passwd 这个文件
	`find / -name "*passwd*"` 找出文件名包含了 passwd 这个关键字的文件
	`find /run -type s` 找出 /run 目录下，文件类型为 Socket 的文件名有哪些？
	`find / -size +1M` 找出系统中，大于 1MB 的文件

磁盘与目录容量

- df：列出文件系统的整体磁盘使用量；
- du：评估文件系统的磁盘使用量（常用在推估目录所占容量）

- `ln from_file dest_file` 创建文件“硬链接”
- `ln -s from_file dest_file` 创建文件“符号链接”，相当于“捷径”。修改链接文件，相当于修改源文件。


### 修改环境变量
`PATH="${PATH}:/root"` 将/root添加到PATH变量

### 软件管理

### 网络


