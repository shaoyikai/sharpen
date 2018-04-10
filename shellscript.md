shell script 是利用 shell 的功能所写的一个“程序 （program）”，这个程序是使用纯文本文件，将一些 shell 语法与指令（含外部指令）写在里面，搭配正则表达式、管线命令与数据流重导向等功能，以达到我们所想要的处理目的。

shell script 可以简单的被看成是批处理文件， 也可以被说成是一个程序语言，且这个程序语言由于都是利用 shell 与相关工具指令，所以不需要编译即可执行，且拥有不错的除错（debug）工具，所以，他可以帮助系统管理员快速的管理好主机。

### 干嘛学习 shell script ？

- 自动化管理的重要依据
- 追踪与管理系统的重要工作
- 简单入侵侦测功能
- 连续指令单一化
- 简易的数据处理
- 跨平台支持与学习历程较短


shell script 用在系统管理上面是很好的一项工具，但是用在处理大量数值运算上， 就不够好了，因为 Shell scripts 的速度较慢，且使用的 CPU 资源较多，造成主机资源的分配不良。

### 乘法计算示例：
```bash
#!/bin/bash

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH

echo -e "You SHOULD input 2 numbers, i will multiplying them! \n "
read -p "first number: " firstnu
read -p "second number: " secnu

declare -i total=${firstnu}*${secnu} # 或者 total=$((${firstnu}*${secnu}))
echo -e "\nThe result of ${firstnu} X ${secnu} is ==> ${total}"

```

### 数值运算：通过 bc 计算 pi
```bash
#!/bin/bash
#filename a.sh
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH

echo -e "This program will calculate pi value. \n"
echo -e "You should input a fload number to calcute pi value. \n"
read -p "The scal number (10~10000) ? " checking
num=${checking:-"10"}
echo -e "Starting calcute pi value. be patient."
time echo "scale=${num};4*a(1)" | bc -lq

```

### 利用 source 来执行脚本：在父程序中执行

其实 script 是在子程序的 bash 内执行的！如果你使用 source 来执行指令那就不一样了

`source a.sh`

`echo $num` <==嘿嘿！有数据产生喔！

这也是为啥你不登出系统而要让某些写入 ~/.bashrc 的设置生效时，需要使用“ source ~/.bashrc ”而不能使用“ bash ~/.bashrc ”是一样的啊！

### 利用 test 指令的测试功能

我要检查 /dmtsai 是否存在时

`test -e /dmtsai && echo "exist" || "not exist"`

淡然 test 还有其他很多参数 
1. 关于某个文件名的“文件类型”判断，如 test -e filename 表示存在否
2. 关于文件的权限侦测，如 test -r filename 表示可读否 （但 root 权限常有例外）
3. 两个文件之间的比较，如： test file1 -nt file2
4. 关于两个整数之间的判定，例如 test n1 -eq n2
5. 判定字串的数据
6. 多重条件判定，例如： test -r filename -a -x filename

练习：
>
	1. 这个文件是否存在，若不存在则给予一个“Filename does not exist”的讯息，并中断程序；
	2. 若这个文件存在，则判断他是个文件或目录，结果输出“Filename is regular file”或 “Filename is directory”
	3. 判断一下，执行者的身份对这个文件或目录所拥有的权限，并输出权限数据！

```bash
#!/bin/bash

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH

echo -e "Please input a filename, I will check the filename's type and permission.\n\n"
read -p "Input a filename: " filename

test -z ${filename} && echo "Your MUST input a filename." && exit 0
test ! -e ${filename} && "The filename '${filename}' DO NOT exist" && exit 0

test -f ${filename} && filetype="regulare file"
test -d ${filename} && filetype="directory"

test -r ${filename} && perm="readable"
test -w ${filename} && perm="${perm} writable"
test -x ${filename} && perm="${perm} executable"

echo "The filename: ${filename} is a ${filetype}"
echo "And the permission for you are: ${perm}"

```
### 利用判断符号 [ ]

除了我们很喜欢使用的 test 之外，其实，我们还可以利用判断符号“ [ ] ”（就是中括号啦） 来进行数据的判断呢！

想要知道 ${HOME} 这个变量是否为空：

`[ -z "${HOME}" ] ; echo $?`

> 使用中括号必须要特别注意，因为中括号用在很多地方，包括万用字符与正则表达式等等，所以如果要在 bash 的语法当中使用中括号作为 shell 的判断式时，必须要注意中括号的两端需要有空白字符来分隔喔！

- 在中括号 [] 内的每个元件都需要有空白键来分隔；
- 在中括号内的变量，最好都以双引号括号起来；
- 在中括号内的常数，最好都以单或双引号括号起来。

```bash
#!/bin/bash

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH

read -p "Please input （Y/N）: " yn
[ "${yn}" == "Y" -o "${yn}" == "y" ] && echo "OK, continue" && exit 0
[ "${yn}" == "N" -o "${yn}" == "n" ] && echo "Oh, interrupt!" && exit 0

echo "I don't know what your choice is" && exit 0
```

### Shell script 的默认变量（$0, $1...）

本章一开始是使用 read 的功能！但 read 功能的问题是你得要手动由键盘输入一些判断式。如果通过指令后面接参数， 那么一个指令就能够处理完毕而不需要手动再次输入一些变量行为！这样下达指令会比较简单方便啦！

- $# ：代表后接的参数“个数”，以上表为例这里显示为“ 4 ”；
- $@ ：代表“ "$1" "$2" "$3" "$4" ”之意，每个变量是独立的（用双引号括起来）；
- $* ：代表“ "$1c$2c$3c$4" ”，其中 c 为分隔字符，默认为空白键， 所以本例中代表“ "$1 $2 $3 $4" ”之意。

```bash
#!/bin/bash

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH

echo "The script name is        ==> ${0}"
echo "Total parameter number is ==> $#"
echo "Your whole parameter is   ==> $@"
echo "The 1st parameter         ==> ${1}"
echo "The 2nd parameter         ==> ${2}"

```

### shift：造成参数变量号码偏移

shift 会移动变量，而且 shift 后面可以接数字，代表拿掉最前面的几个参数的意思。

### 条件判断式

方式一：
>
	if [ 条件判断式 ];then
		指令工作;
	fi

```bash
#!/bin/bash

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH

read -p "Please input （Y/N）: " yn

if [ "${yn}" == "Y" ] || [ "${yn}" == "y" ]; then
	echo "OK, continue"
	exit 0
fi
if [ "${yn}" == "N" ] || [ "${yn}" == "n" ]; then
	echo "Oh, interrupt!"
	exit 0
fi
echo "I don't know what your choice is" && exit 0
```

方式二：
>
	if [ 条件判断式 ]; then
		当条件判断式成立时，可以进行的指令工作内容；
	else
		当条件判断式不成立时，可以进行的指令工作内容；
	fi

方式三：
>
	if [ 条件判断式一 ]; then
		当条件判断式一成立时，可以进行的指令工作内容；
	elif [ 条件判断式二 ]; then
		当条件判断式二成立时，可以进行的指令工作内容；
	else
		当条件判断式一与二均不成立时，可以进行的指令工作内容；
	fi	

```bash


检测服务器上安装的服务脚本
#!/bin/bash

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH

echo "Now, I will detect your Linux server's services!"

testfile=/dev/shm/netstat_checking.txt
netstat -tuln > ${testfile}
testing=$(grep ":80 " ${testfile})
if [ "${testing}" == "" ];then
        echo -e "\033[31m[error]\033[0m: WWW"
else
        echo -e "\033[32m[ok]\033[0m: www"
fi
testing=$(grep ":22 " ${testfile})
if [ "${testing}" == "" ];then
        echo -e "\033[31m[error]\033[0m: SSH"
else
        echo -e "\033[32m[ok]\033[0m: ssh"
fi
testing=$(grep ":3306 " ${testfile})
if [ "${testing}" == "" ];then
        echo -e "\033[31m[error]\033[0m: Mysql"
else
        echo -e "\033[32m[ok]\033[0m: mysql"
fi
testing=$(grep ":9000 " ${testfile})
if [ "${testing}" == "" ];then
        echo -e "\033[31m[error]\033[0m: PHP-FPM"
else
        echo -e "\033[32m[ok]\033[0m: php-fpm"
fi
```

### 利用 case ..... esac 判断

每一个变量内容的程序段最后都需要两个分号 （;;） 来代表该程序段落的结束，这挺重要的喔！ 至于为何需要有 * 这个变量内容在最后呢？这是因为，如果使用者不是输入变量内容一或二时， 我们可以告知使用者相关的信息啊！

```bash
#!/bin/bash

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH

case ${1} in 
        "hello")
                echo "Hello,how are you?"
                ;;
        "")
                echo "You MUST input parameters, ex > {${0} someword}"
                ;;
        *)
                echo "Usage ${0} {hello}"
                ;;
esac
```


### 利用 function 功能

因为 shell script 的执行方式是由上而下，由左而右， 因此在 shell script 当中的function的设置一定要在程序的最前面，这样才能够在执行时被找到可用的程序段喔

### 循环 （loop）

当 condition 条件成立时，就进行循环，直到 condition 的条件不成立才停止
>
	while [ condition ] 
	do 
		程序段落
	done 

当 condition 条件成立时，就终止循环， 否则就持续进行循环的程序段。
>
	until [ condition ]
	do
		程序段落
	done

```bash
#!/bin/bash

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
s=0 
i=0 
while [ "${i}" != "100" ]
do
i=$(($i+1))
s=$(($s+$i)) 
done
echo "The result of '1+2+3+...+100' is ==> $s"
```

### for...do...done （固定循环）
>
	for var in con1 con2 con3 ...
	do
		程序段
	done

显示出 192.168.1.1~192.168.1.20 共 20 部主机目前是否能与你的机器连通！
```bash
#!/bin/bash

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH

network="192.168.1"

for sitenu in $(seq 1 20)
do
        ping -c 1 -w 1 ${network}.${sitenu} &> /dev/null && result=0 || result=1
        if [ "${result}" == 0 ]; then
                echo "Server ${network}.${sitenu} is UP."
        else
                echo "Server ${network}.${sitenu} is DOWN."
        fi
done

```

### for...do...done 的数值处理

>
	for （（ 初始值; 限制值; 执行步阶 ））
	do
		程序段
	done

### shell ssccrriipptt 的追踪与 debug

sh [-nvx] scripts.sh

选项与参数：
-n ：不要执行 script，仅查询语法的问题；
-v ：再执行 sccript 前，先将 scripts 的内容输出到屏幕上；
-x ：将使用到的 script 内容显示到屏幕上，这是很有用的参数！

### 账号管理

### 新增与移除使用者： useradd, 相关配置文件, passwd, usermod, userdel

要如何在 Linux 的系统新增一个使用者啊？呵呵～真是太简单了～我们登陆系统时会输入 （1）帐号与 （2）密码， 所以创建一个可用的帐号同样的也需要这两个数据。

`useradd vbird1`

其实系统已经帮我们规定好非常多的默认值了，所以我们可以简单的使用“ useradd 帐号 ”来创建使用者即可。 CentOS 这些默认值主要会帮我们处理几个项目：
- 在 /etc/passwd 里面创建一行与帐号相关的数据，包括创建 UID/GID/主文件夹等；
- 在 /etc/shadow 里面将此帐号的密码相关参数填入，但是尚未有密码；
- 在 /etc/group 里面加入一个与帐号名称一模一样的群组名称；
- 在 /home 下面创建一个与帐号同名的目录作为使用者主文件夹，且权限为 700

由于在 /etc/shadow 内仅会有密码参数而不会有加密过的密码数据，因此我们在创建使用者帐号时， 还需要使用“ passwd 帐号 ”来给予密码才算是完成了使用者创建的流程。


