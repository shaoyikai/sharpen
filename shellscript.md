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

利用判断符号 [ ]

530