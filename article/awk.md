awk 也是一个非常棒的数据处理工具！相较于 sed 常常作用于一整个行的处理， awk 则比较倾向于一行当中分成数个“字段”来处理。

awk '条件类型1{动作1} 条件类型2{动作2} ...' filename

awk 后面接两个单引号并加上大括号 {} 来设置想要对数据进行的处理动作。 awk 可以处理后续接的文件，也可以读取来自前个指令的 standard output 。但如前面说的， awk 主要是处理“每一行的字段内的数据”，而默认的“字段的分隔符号为 "空白键" 或 "[tab]键" ”！

`last -n 5`

`last -n 5 | awk '{print $1 "\t" $3}'`

由上面这个例子你也会知道，在 awk 的括号内，每一行的每个字段都是有变量名称的，那就是 $1, $2... 等变量名称。

awk 的内置变量：
- NF 每一行 （$0） 拥有的字段总数
- NR 目前 awk 所处理的是“第几行”数据
- FS 目前的分隔字符，默认是空白键

### awk 的逻辑运算字符
>
	> 大于
	< 小于
	>= 大于或等于
	<= 小于或等于
	== 等于
	!= 不等于

举例来说，在 /etc/passwd 当中是以冒号 ":" 来作为字段的分隔， 该文件中第一字段为帐号，第三字段则是 UID。那假设我要查阅，第三栏小于 10 以下的数据，并且仅列出帐号与第三栏

`cat /etc/passwd | awk '{FS=":"} $3 < 10 {print $1 "\t " $3}'`

awk 的输出格式当中，常常会以 printf 来辅助，所以， 最好你对 printf 也稍微熟悉一下比较好啦！另外， awk 的动作内 {} 也是支持 if （条件） 的喔！