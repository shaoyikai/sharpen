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
	----------------------
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

### 软件管理

### 网络


