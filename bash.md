《鸟哥linux私房菜基础学习篇》读书笔记
------


### 变量

- myname=yikai

示例
> 
	var="lang is $LANG"
	echo $var

> lang is zh_CN.UTF-8

> 
	version=$(uname -r)
	echo $version

> 3.10.0-229.el7.x86_64

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

> \e[0;32m[\u@\h \W]$ \e[m

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

read 之后不加任何参数，直接加上变量名称，那么下面就会主动出现一个空白行等待你的输入（如范例一）。 如果加上 -
t 后面接秒数，例如上面的范例二，那么 30 秒之内没有任何动作时， 该指令就会自动略过了～如果是加上 -p ，嘿嘿！在输入的光
标前就会有比较多可以用的提示字符给我们参考！ 在指令的下达里面，比较美观啦！ ^_^