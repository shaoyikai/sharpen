sed 本身也是一个管线命令，可以分析 standard input 的啦！ 而且 sed 还可以将数据进行取 代、删除、新增、撷取特定行等等的功能呢！

sed [-nefr] [动作]
>
	-n ：使用安静（silent）模式。在一般 sed 的用法中，所有来自 STDIN 的数据一般都会被列出到屏幕上。
	但如果加上 -n 参数后，则只有经过 sed 特殊处理的那一行（或者动作）才会被列出来。
	-e ：直接在命令行界面上进行 sed 的动作编辑；
	-f ：直接将 sed 的动作写在一个文件内， -f filename 则可以执行 filename 内的 sed 动作；
	-r ：sed 的动作支持的是延伸型正则表达式的语法。（默认是基础正则表达式语法）
	-i ：直接修改读取的文件内容，而不是由屏幕输出。

动作说明： [n1[,n2]]function 

n1, n2 ：不见得会存在，一般代表“选择进行动作的行数”，举例来说，如果我的动作是需要在 10 到 20 行之间进行的，则“ 10,20[动作行为] ”

function 有下面这些咚咚：
>		
	a ：新增， a 的后面可以接字串，而这些字串会在新的一行出现（目前的下一行）～
	c ：取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
	d ：删除，因为是删除啊，所以 d 后面通常不接任何咚咚；
	i ：插入， i 的后面可以接字串，而这些字串会在新的一行出现（目前的上一行）；
	p ：打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～
	s ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正则表达式！

将 /etc/passwd 的内容列出并且打印行号，同时，请将第 2~5 行删除！sed 后面接的动作，请务必以 '' 两个单引号括住喔！

`nl /etc/passwd | sed '2,5d'`

如果只要删除第 2 行，可以使用

`nl /etc/passwd | sed '2d'`

若是要删除第 3 到最后一行

`nl /etc/passwd | sed '3,$d'`

> 那个钱字号“ $ ”代表最后一行！

在第二行后（亦即是加在第三行）加上“drink tea?”字样！

`nl /etc/passwd | sed '2a drink tea'`

在第二行后面加入两行字，例如“Drink tea or .....”与“drink beer?”

`nl /etc/passwd | sed '2a Drink tea or ......\
> drink beer ?'`

### 以行为单位的取代与显示功能

刚刚是介绍如何新增与删除，那么如果要整行取代呢？

我想将第2-5行的内容取代成为“No 2-5 number”呢？

`nl /etc/passwd | sed '2,5c No 2-5 number'`

仅列出 /etc/passwd 文件内的第 5-7 行

`nl /etc/passwd | sed -n '5,7p'`  <===> `head -n 20 | tail -n 10`

### 部分数据的搜寻并取代的功能

除了整行的处理模式之外， sed 还可以用行为单位进行部分数据的搜寻并取代的功能喔！ 基本上 sed 的搜寻与取代的与 vi 相当的类似！

sed 's/要被取代的字串/新的字串/g'

先使用 grep 将关键字 MAN 所在行取出来

`cat /etc/man_db.conf | grep 'MAN'`

删除掉注解之后的数据！

`cat /etc/man_db.conf | grep 'MAN' | sed 's/#.*$//g'`

从上面可以看出来，原本注解的数据都变成空白行啦！所以，接下来要删除掉空白行

`cat /etc/man_db.conf | grep 'MAN'| sed 's/#.*$//g' | sed '/^$/d'`

### 直接修改文件内容（危险动作）

请你千万不要随便拿系统配置文件来测试喔！ 我们还是使用你下载的 regular_express.txt 文件来测试看看吧！

利用 sed 将 regular_express.txt 内每一行结尾若为 . 则换成 !

`sed -i 's/\.$/\!/g' regular_express.txt`

利用 sed 直接在 regular_express.txt 最后一行加入“# This is a test”

`sed -i '$a #This is a test' regular_express.txt`