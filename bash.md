《鸟哥linux私房菜基础学习篇》读书笔记
------


### 变量

- myname=yikai

示例
> 
	var="lang is $LANG"
	echo $var
	#> lang is zh_CN.UTF-8

> 
	version=$(uname -r)
	echo $version
	#> 3.10.0-229.el7.x86_64

修改变量

>
	PATH="$PATH":/home/bin
	PATH=${PATH}:/home/bin

将变量变为环境变量

>
	name=yikai
	bash
	echo $name # 此时为空
	exit
	export name 
	bash
	echo $name # yikai
	exit

> 什么是“子程序”呢？就是说，在我目前这个 shell 的情况下，去启用另一个新的 shell ，新的那个 shell 就是子程序啦！在一般的状态下，父程序的自订变量是无法在子程序内使用的。但是通过 export 将变量变成环境变量后，就能够在子程序下面应用了！

取消变量 unset

>
	name=yikai
	echo $name
	unset name
	echo $name

### 环境变量 env

$RANDOM 来获取随机数，范围 0-32767

随机取出 0~9 之间的数值喔

> 
	declare -i number=$RANDOM*10/32768 ; echo $number

### 用 set 观察所有变量 （含环境变量与自订变量）

>
	echo $PS1
	#> \e[0;32m[\u@\h \W]$ \e[m

PS1：（提示字符的设置）
>
	\d ：可显示出“星期 月 日”的日期格式，如："Mon Feb 2"
	\H ：完整的主机名称。举例来说，鸟哥的练习机为“study.centos.vbird”
	\h ：仅取主机名称在第一个小数点之前的名字，如鸟哥主机则为“study”后面省略
	\t ：显示时间，为 24 小时格式的“HH:MM:SS”
	\T ：显示时间，为 12 小时格式的“HH:MM:SS”
	\A ：显示时间，为 24 小时格式的“HH:MM”
	\@ ：显示时间，为 12 小时格式的“am/pm”样式
	\u ：目前使用者的帐号名称，如“dmtsai”；
	\v ：BASH 的版本信息，如鸟哥的测试主机版本为 4.2.46（1）-release，仅取“4.2”显示
	\w ：完整的工作目录名称，由根目录写起的目录名称。但主文件夹会以 ~ 取代；
	\W ：利用 basename 函数取得工作目录名称，所以仅会列出最后一个目录名。
	\# ：下达的第几个指令。
	\$ ：提示字符，如果是 root 时，提示字符为 # ，否则就是 $ 啰～

$：（关于本 shell 的 PID）

> 钱字号本身也是个变量喔！这个咚咚代表的是“目前这个 Shell 的线程代号”，亦即是所谓的 PID （Process ID）。
>
	echo $$

?：（关于上个执行指令的回传值）
> 一般来说，如果成功的执行该指令， 则会回传一个 0 值，如果执行过程发生错误，就会回传“错误代码”才对！一般就是以非为 0 的数值来取代。


export： 自订变量转成环境变量

影响显示结果的语系变量 （locale），相关文件 /etc/locale.conf

>
	locale -a

### 变量键盘读取、阵列与宣告： read, array, declare

我们上面提到的变量设置功能，都是由命令行直接设置的，那么，可不可以让使用者能够经由键盘输入？ 什么意思呢？
是否记得某些程序执行的过程当中，会等待使用者输入 "yes/no" 之类的讯息啊？ 在 bash 里面也有相对应的功能喔！此外，我们还可以宣告这个变量的属性，例如：阵列或者是数字等等的。

read [-pt] variable
>
	选项与参数：
	-p ：后面可以接提示字符！
	-t ：后面可以接等待的“秒数！”这个比较有趣～不会一直等待使用者啦！

例如
>
	read atest # 输入“this is a test”
	echo ${atest} # 输出“this is a test”

>
	read -p "Please keyin your name: " -t 30 named

read 之后不加任何参数，直接加上变量名称，那么下面就会主动出现一个空白行等待你的输入（如范例一）。 如果加上 - t 后面接秒数，例如上面的范例二，那么 30 秒之内没有任何动作时，该指令就会自动略过了～如果是加上 -p ，嘿嘿！在输入的光标前就会有比较多可以用的提示字符给我们参考！ 在指令的下达里面，比较美观啦！


declare 或 typeset 是一样的功能，就是在“宣告变量的类型”。如果使用 declare 后面并没有接任何参数，那么 bash 就会主
动的将所有的变量名称与内容通通叫出来，就好像使用 set 一样啦！

declare [-aixr] variable
选项与参数：
>
	-a ：将后面名为 variable 的变量定义成为阵列 （array） 类型
	-i ：将后面名为 variable 的变量定义成为整数数字 （integer） 类型
	-x ：用法与 export 一样，就是将后面的 variable 变成环境变量；
	-r ：将变量设置成为 readonly 类型，该变量不可被更改内容，也不能 unset

举例
>
	declare -i sum=100+300+50
	echo ${sum} # 450

array 变量类型
>
	var[1]='small min'
	var[2]='big min'
	var[3]='nice min'
	echo "${var[1]},${var[2]},${var[3]}" # small min,big min,nice min

### 与文件系统及程序的限制关系： ulimit

bash 是可以“限制使用者的某些系统资源”的，包括可以打开的文件
数量， 可以使用的 CPU 时间，可以使用的内存总量等等。如何设置？用 ulimit 吧！

ulimit [-SHacdfltu] [配额]

选项与参数：
>
	-H ：hard limit ，严格的设置，必定不能超过这个设置的数值；
	-S ：soft limit ，警告的设置，可以超过这个设置值，但是若超过则有警告讯息。
	在设置上，通常 soft 会比 hard 小，举例来说，soft 可设置为 80 而 hard
	设置为 100，那么你可以使用到 90 （因为没有超过 100），但介于 80~100 之间时，
	系统会有警告讯息通知你！
	-a ：后面不接任何选项与参数，可列出所有的限制额度；
	-c ：当某些程序发生错误时，系统可能会将该程序在内存中的信息写成文件（除错用），
	这种文件就被称为核心文件（core file）。此为限制每个核心文件的最大容量。
	-f ：此 shell 可以创建的最大文件大小（一般可能设置为 2GB）单位为 KBytes
	-d ：程序可使用的最大断裂内存（segment）容量；
	-l ：可用于锁定 （lock） 的内存量
	-t ：可使用的最大 CPU 时间 （单位为秒）
	-u ：单一使用者可以使用的最大程序（process）数量。

举例
>
	ulimit -f 10240 
	ulimit -a | grep 'file size'

变量内容的删除与取代

`${variable#/*local/bin:}`

- 这就是原本的变量名称，以上面范例二来说，这里就填写 path 这个“变量名称”啦！
- `#`是重点！代表“从变量内容的最前面开始向右删除”，且仅删除最短的那个
- `/*local/bin:`代表要被删除的部分，由于 # 代表由前面开始删除，所以这里便由开始的 / 写起。需要注意的是，我们还可以通过万用字符 * 来取代 0 到无穷多个任意字符

举例
>
	path=${PATH}
	echo ${path}
	#> /usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin
	echo ${path#/*local/bin:}
	#> /usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin

举例
>
	# 我想要删除前面所有的目录，仅保留最后一个目录
	echo ${path#/*:}
	# 由于一个`#`仅删除掉最短的那个，因此他删除的情况可以用下面的删除线来看：
	#> /usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin
	echo ${path##/*:}
	#> /home/dmtsai/bin
	#嘿！多加了一个 # 变成 ## 之后，他变成“删除掉最长的那个数据”！

- # ：符合取代文字的“最短的”那一个；
- ##：符合取代文字的“最长的”那一个

上面谈到的是“从前面开始删除变量内容”，那么如果想要“从后面向前删除变量内容”呢？ 这个时候就得使用百分比 （%）符号了！
>
	#我想要删除最后面那个目录，亦即从 : 到 bin 为止的字串
	echo ${path%:*bin}
	#> /usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin
	# 注意啊！最后面一个目录不见去！
	# 这个 % 符号代表由最后面开始向前删除！
	echo ${path%%:*bin}
	#> /usr/local/bin

了解了删除功能后，接下来谈谈取代吧！
>
	# 将 path 的变量内容内的 sbin 取代成大写 SBIN：
	echo ${path/sbin/SBIN}
	#> /usr/local/bin:/usr/bin:/usr/local/SBIN:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin
	# 这个部分就容易理解的多了！关键字在于那两个斜线，两斜线中间的是旧字串
	# 后面的是新字串，所以结果就会出现如上述的特殊字体部分啰！
	echo ${path//sbin/SBIN}
	#> /usr/local/bin:/usr/bin:/usr/local/SBIN:/usr/SBIN:/home/dmtsai/.local/bin:/home/dmtsai/bin
	# 如果是两条斜线，那么就变成所有符合的内容都会被取代喔！

总结
- ${变量#关键字} 若变量内容从头开始的数据符合“关键字”，则将符合的最短数据删除
- ${变量##关键字} 若变量内容从头开始的数据符合“关键字”，则将符合的最长数据删除
- ${变量%关键字} 若变量内容从尾向前的数据符合“关键字”，则将符合的最短数据删除
- ${变量%%关键字} 若变量内容从尾向前的数据符合“关键字”，则将符合的最长数据删除
- ${变量/旧字串/新字串} 若变量内容符合“旧字串”则“第一个旧字串会被新字串取代”
- ${变量//旧字串/新字串} 若变量内容符合“旧字串”则“全部的旧字串会被新字串取代”

变量的测试与内容替换

在某些时刻我们常常需要“判断”某个变量是否存在，若变量存在则使用既有的设置，若变量不存在则给予一个常用的设置。

new_var=${old_var-content}
>
	old_var 旧的变量
	content 如果old_var 未设置，则使用content的值
	username=""
	username=${username-root}
	echo ${username}
	# 因为 username 被设置为空字串了！所以当然还是保留为空字串！
	username=${username:-root}
	echo ${username}
	root <==加上“ : ”后若变量内容为空或者是未设置，都能够以后面的内容替换！

> 在大括号内有没有冒号“ : ”的差别是很大的！加上冒号后，被测试的变量未被设置或者是已被设置为空字串时， 都能够用后面的内容 （本例中是使用 root 为内容） 来替换与设置！

### history 命令

- history 列出所有历史命令
- history 3 列出最近的3条历史命令
- history -c 删除所有history记录
- history -w 立刻将目前的数据写入 ~/.bash_history 当中

那么 history 这个历史命令只可以让我查询命令而已吗？呵呵！当然不止啊！ 我们可以利用相关的功能来帮我们执行命令呢！
>
	!number 执行第几笔指令的意思
	!command 由最近的指令向前搜寻“指令串开头为 command”的那个指令，并执行；
	!! 就是执行上一个指令

### bash shell 的操作环境

bash 的进站与欢迎讯息： /etc/issue, /etc/motd

issue 内的各代码意义
- \d 本地端时间的日期；
- \l 显示第几个终端机接口；
- \m 显示硬件的等级 （i386/i486/i586/i686...）；
- \n 显示主机的网络名称；
- \O 显示 domain name；
- \r 操作系统的版本 （相当于 uname -r）
- \t 显示本地端时间的时间；
- \S 操作系统的名称；
- \v 操作系统的版本。

### bash 的环境配置文件

- login shell：取得 bash 时需要完整的登陆流程的，就称为 login shell。举例来说，你要由 tty1 ~ tty6 登陆，需要输入使用者的帐号与密码，此时取得的 bash 就称为“ login shell ”啰；
- non-login shell：取得 bash 接口的方法不需要重复登陆的举动，举例来说，（1）你以 X window 登陆 Linux 后， 再以 X 的图形化接口启动终端机，此时那个终端接口并没有需要再次的输入帐号与密码，那个 bash 的环境就称为 non-login shell了。（2）你在原本的 bash 环境下再次下达 bash 这个指令，同样的也没有输入帐号密码， 那第二个 bash （子程序） 也是 non-login shell 。

login shell 其实只会读取这两个配置文件：
1. /etc/profile：这是系统整体的设置，你最好不要修改这个文件；
2. ~/.bash_profile 或 ~/.bash_login 或 ~/.profile：属于使用者个人设置，你要改自己的数据，就写入这里！

### source ：读入环境配置文件的指令

由于 /etc/profile 与 ~/.bash_profile 都是在取得 login shell 的时候才会读取的配置文件，所以， 如果你将自己的偏好设置写
入上述的文件后，通常都是得登出再登陆后，该设置才会生效。那么，能不能直接读取配置文件而不登出登陆呢？ 可以的！那就得
要利用 source 这个指令了！

source 配置文件文件名

范例：将主文件夹的 ~/.bashrc 的设置读入目前的 bash 环境中
>
	source ~/.bashrc <==下面这两个指令是一样的！
	. ~/.bashrc

利用 source 或小数点 （.） 都可以将配置文件的内容读进来目前的 shell 环境中！举例来说，我修改了 ~/.bashrc ，那么
不需要登出，立即以 source ~/.bashrc 就可以将刚刚最新设置的内容读进来目前的环境中！

~/.bashrc （non-login shell 会读）

### 其他相关配置文件

/etc/man_db.conf 规范了使用 man 的时候， man page 的路径到哪里去寻找！

~/.bash_history 。每次登陆 bash 后，bash 会先读取这个文件，将所有的历史指令读入内存，
因此，当我们登陆 bash 后就可以查知上次使用过哪些指令啰。

~/.bash_logout 当我登出 bash 后，系统再帮我做完什么动作后才离开。默认的情况下，登出时， bash 只是帮我们清掉屏幕的讯息而已。 不过，你也可以将一些备份或者是其他你认为重要的工
作写在这个文件中 （例如清空暂存盘）， 那么当你离开 Linux 的时候，就可以解决一些烦人的事情啰！

### 终端机的环境设置： stty, set

我们可以利用 stty -a 来列出目前环境中所有的按键列表，在上头的列表当中，需要注意的是特殊字体那几个， 此外，如果出现 ^ 表示 [Ctrl] 那个按键的意思。举例来说， intr = ^C 表示利用 [ctrl] + c 来达成的。

列出所有的按键与按键内容 `setty -a`
>
	intr : 送出一个 interrupt （中断） 的讯号给目前正在 run 的程序 （就是终止啰！）；
	quit : 送出一个 quit 的讯号给目前正在 run 的程序；
	erase : 向后删除字符，
	kill : 删除在目前命令行上的所有文字；
	eof : End of file 的意思，代表“结束输入”。
	start : 在某个程序停止后，重新启动他的 output
	stop : 停止目前屏幕的输出；
	susp : 送出一个 terminal stop 的讯号给正在 run 的程序。

如果你想要用 [ctrl]+h 来进行字符的删除，那么可以下达：
>
	stty erase ^h # 这个设置看看就好，不必真的实做！不然还要改回来！

那么从此之后，你的删除字符就得要使用 [ctrl]+h 啰，按下 [backspace] 则会出现 ^? 字样呢！ 如果想要回复利用
[backspace] ，就下达 stty erase ^? 即可啊！ 至于更多的 stty 说明，记得参考一下 man stty 的内容喔！


除了 stty 之外，其实我们的 bash 还有自己的一些终端机设置值呢！那就是利用 set 来设置的！set 可以帮我们设置整个指令输出/输入的环境。 例如记录历史命令、显示错误内容等
等。

事实上，并不建议您修改 tty 的环境，这是因为 bash 的环境已经设置的很友好了， 我们不需要额外的设置或者修改，否则反而会产生一些困扰。

最后，我们将 bash 默认的组合键给他汇整如下
- Ctrl + C 终止目前的命令
- Ctrl + D 输入结束 （EOF），例如邮件结束的时候；
- Ctrl + M 就是 Enter 啦！
- Ctrl + S 暂停屏幕的输出
- Ctrl + Q 恢复屏幕的输出
- Ctrl + U 在提示字符下，将整列命令删除
- Ctrl + Z “暂停”目前的命令


### 万用字符与特殊符号
- * 代表“ 0 个到无穷多个”任意字符
- ? 代表“一定有一个”任意字符
- [] 同样代表“一定有一个在括号内”的字符（非任意字符）。例如 [abcd] 代表“一定有一个字符， 可能是 a, b, c, d 这四个任
何一个”
- [-] 若有减号在中括号内时，代表“在编码顺序内的所有字符”。例如 [0-9] 代表 0 到 9 之间的所有数字，因为数字的语系编码是连续的！
- [^] 若中括号内的第一个字符为指数符号 （^） ，那表示“反向选择”，例如 [^abc] 代表 一定有一个字符，只要是非 a, b, c的其他字符就接受的意思。

举例
>
	ll -d /etc/cron* <==以 cron 为开头的文件名
	ll -d /etc/????? <==文件名“刚好是五个字母”的文件名
	ll -d /etc/*[0-9]* <==文件名含有数字的文件名
	ll -d /etc/[^a-z]* <==文件名开头非为小写字母的文件名：
	mkdir /tmp/upper; cp -a /etc/[^a-z]* /tmp/upper <==找到的文件复制到 /tmp/upper 中

除了万用字符之外，bash 环境中的特殊符号有哪些呢？下面我们先汇整一下：
- # 注解符号：这个最常被使用在 script 当中，视为说明！在后的数据均不执行
- \ 跳脱符号：将“特殊字符或万用字符”还原成一般字符
- | 管线 （pipe）：分隔两个管线命令的界定（后两节介绍）；
- ; 连续指令下达分隔符号：连续性命令的界定 （注意！与管线命令并不相同）
- ~	使用者的主文件夹
- $ 取用变量前置字符：亦即是变量之前需要加的变量取代值
- & 工作控制 （job control）：将指令变成背景下工作
- ! 逻辑运算意义上的“非” not 的意思！
- / 目录符号：路径分隔的符号
- >,>> 数据流重导向：输出导向，分别是“取代”与“累加”
- <,<< 数据流重导向：输入导向 （这两个留待下节介绍）
- '' 单引号，不具有变量置换的功能 （$ 变为纯文本）
- "" 具有变量置换的功能！ （$ 可保留相关功能）
- `` 两个“ ` ”中间为可以先执行的指令，亦可使用 $（ ）
- () 在中间为子 shell 的起始与结束
- {} 在中间为命令区块的组合！

> 理论上，你的“文件名”尽量不要使用到上述的字符啦！

### 数据流重导向
我们执行一个指令的时候，这个指令可能会由文件读入数据，经过处理之后，再将数据输出到屏幕上。

standard output 与 standard error output

简单的说，标准输出指的是“指令执行所回传的正确的讯息”，而标准错误输出可理解为“ 指令执行失败后，所回传的错误讯息”。

1. 标准输入　　（stdin） ：代码为 0 ，使用 < 或 << ；
2. 标准输出　　（stdout）：代码为 1 ，使用 > 或 >> ；
3. 标准错误输出（stderr）：代码为 2 ，使用 2> 或 2>> ；

- 1> ：以覆盖的方法将“正确的数据”输出到指定的文件或设备上；
- 1>>：以累加的方法将“正确的数据”输出到指定的文件或设备上；
- 2> ：以覆盖的方法将“错误的数据”输出到指定的文件或设备上；
- 2>>：以累加的方法将“错误的数据”输出到指定的文件或设备上；

例如执行“ find / -name testing ”时，可能会产生类似“find: /root: Permission denied”之类的讯息。

stdout 与 stderr 分存到不同的文件去
>
	find /home -name .bashrc > list_right 2> list_error

### /dev/null 垃圾桶黑洞设备与特殊写法
想像一下，如果我知道错误讯息会发生，所以要将错误讯息忽略掉而不显示或储存呢？ 这个时候黑洞设备 /dev/null 就很重要了！这个 /dev/null 可以吃掉任何导向这个设备的信息喔！

将错误的数据丢弃，屏幕上显示正确的数据
>
	find /home -name .bashrc 2> /dev/null

再想像一下，如果我要将正确与错误数据通通写入同一个文件去呢？这个时候就得要使用特殊的写法了！
>
	find /home -name .bashrc > list 2> list <==错误
	find /home -name .bashrc > list 2>&1 <==正确
	find /home -name .bashrc &> list <==正确

上述表格第一行错误的原因是，由于两股数据同时写入一个文件，又没有使用特殊的语法， 此时两股数据可能会交叉写入该文件内，造成次序的错乱。所以虽然最终 list 文件还是会产生，但是里面的数据排列就会怪怪的，而不是原本屏幕上的输出排序。 至于写入同一个文件的特殊语法如上表所示，你可以使用 2>&1 也可以使用 &> ！ 一般来说，鸟哥比较习惯使用 2>&1 的语法啦！

### standard input ： < 与 <<

将原本需要由键盘输入的数据，改由文件内容来取代

用 stdin 取代键盘的输入以创建新文件的简单流程
>
	cat > catfile < ~/.bashrc

### 管线命令 （ppiippee）

bash 命令执行的时候有输出的数据会出现！ 那么如果这群数据必需要经过几道手续之后才能得到我
们所想要的格式，应该如何来设置？ 这就牵涉到管线命令的问题了 （pipe） ，管线命令使用的是“ | ”这个界定符号！

> 
	ls -al /etc | less

其实这个管线命令“ | ”仅能处理经由前面一个指令传来的正确信息，也就是 standard output 的信息，对于 stdandard error 并没有直接处理的能力。	

在每个管线后面接的第一个数据必定是“指令”喔！而且这个指令必须要能够接受 standard input 的数据才行，这样的指令才可以是为“管线命令”，例如 less, more, head, tail 等都是可以接受 standard input 的管线命令啦。至于例如 ls, cp, mv 等就不是管线命令了！因为 ls, cp, mv 并不会接受来自 stdin 的数据。

### 撷取命令： cut, grep

cut -d'分隔字符' -f fields <==用于有特定分隔字符

选项与参数：
>
	-d ：后面接分隔字符。与 -f 一起使用；
	-f ：依据 -d 的分隔字符将一段讯息分区成为数段，用 -f 取出第几段的意思；
	-c ：以字符 （characters） 的单位取出固定字符区间；

>
	echo ${PATH}
	#> /usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin
	echo ${PATH} | cut -d ':' -f 3,5 <===列出第 3 与第 5 呢

将 export 输出的讯息，取得第 12 字符以后的所有字串
>
	export
	#> declare -x HISTCONTROL="ignoredups"
	#> declare -x HISTSIZE="1000"
	#> declare -x HOME="/home/dmtsai"
	#> declare -x HOSTNAME="study.centos.vbird"
	export | cut -c 12-
	#> HISTCONTROL="ignoredups"
	#> HISTSIZE="1000"
	#> HOME="/home/dmtsai"
	#> HOSTNAME="study.centos.vbird"

cut 主要的用途在于将“同一行里面的数据进行分解！”最常使用在分析一些数据或文字数据的时候！ 不过，cut 在处理多空格相连的数据时，可能会比较吃力一点，所以某些时刻可能会使用下一章的 awk 来取代的！


刚刚的 cut 是将一行讯息当中，取出某部分我们想要的，而 grep 则是分析一行讯息， 若当中有我们所需要的信息。

grep [-acinv] [--color=auto] '搜寻字串' filename

选项与参数：
>
	-a ：将 binary 文件以 text 文件的方式搜寻数据
	-c ：计算找到 '搜寻字串' 的次数
	-i ：忽略大小写的不同，所以大小写视为相同
	-n ：顺便输出行号
	-v ：反向选择，亦即显示出没有 '搜寻字串' 内容的那一行！
	--color=auto ：可以将找到的关键字部分加上颜色的显示喔！

举例
>
	last | grep 'root' <===有出现 root 的那一行就取出来；
	last | grep -v 'root' <===只要没有 root 的就取出！
	last | grep 'root' |cut -d ' ' -f1 <===在取出 root 之后，利用上个指令 cut 的处理，就能够仅取得第一栏啰！


### 排序命令： sort, wc, uniq

sort [-fbMnrtuk] [file or stdin]
选项与参数：
>
	-f ：忽略大小写的差异，例如 A 与 a 视为编码相同；
	-b ：忽略最前面的空白字符部分；
	-M ：以月份的名字来排序，例如 JAN, DEC 等等的排序方法；
	-n ：使用“纯数字”进行排序（默认是以文字体态来排序的）；
	-r ：反向排序；
	-u ：就是 uniq ，相同的数据中，仅出现一行代表；
	-t ：分隔符号，默认是用 [tab] 键来分隔；
	-k ：以那个区间 （field） 来进行排序的意思

举例
>
	cat /etc/passwd | sort -t ':' -k 3 <===以第三栏来排序

uniq [-ic]
选项与参数：
>
	-i ：忽略大小写字符的不同；
	-c ：进行计数

举例
>
	last | cut -d ' ' -f1 | sort | uniq <===进行排序后仅取出一位
	last | cut -d ' ' -f1 | sort | uniq -c <===如果我还想要知道每个人的登陆总次数

wc

如果我想要知道 /etc/man_db.conf 这个文件里面有多少字？多少行？多少字符的话，可以怎么做呢？其实可以利用 wc 这 个指令来达成喔！他可以帮我们计算输出的讯息的整体数据！

wc [-lwm]

选项与参数：
>
	-l ：仅列出行；
	-w ：仅列出多少字（英文单字）；
	-m ：多少字符；

>
	cat /etc/man_db.conf | wc
	#> 131 723 5171 <===行、字数、字符数