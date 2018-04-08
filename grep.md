
grep 在数据中查寻一个字串时，是以 "整行" 为单位来进行数据的撷取的！

假如有文件 regular_express.txt 内容如下：

```txt
"Open Source" is a good mechanism to develop programs.
apple is my favorite food.
Football game is not use feet only.
this dress doesn't fit me.
However, this dress is about $ 3183 dollars.^M
GNU is free air not free beer.^M
Her hair is very beauty.^M
I can't finish the test.^M
Oh! The soup taste good.^M
motorcycle is cheap than car.
This window is clear.
the symbol '*' is represented as start.
Oh! My god!
The gd software is a library for drafting programs.^M
You are the best is mean you are the no. 1.
The world <Happy> is the same with "glad".
I like dog.
google is the best tools for search keyword.
goooooogle yes!
go! go! Let's go.
# I am VBird

```

### 搜寻特定字串

假设我们要从刚刚的文件当中取得 the 这个特定字串

`grep -n 'the' regular_express.txt`

那如果想要“反向选择”呢？也就是说，当该行没有 'the' 这个字串时才显示在屏幕上

`grep -vn 'the' regular_express.txt`

如果你想要取得不论大小写的 the 这个字串

`grep -in 'the' regular_express.txt`

### 利用中括号 [] 来搜寻集合字符

如果我想要搜寻 test 或 taste 这两个单字时，可以发现到，其实她们有共通的 't?st' 存在

`grep -n 't[ae]st' regular_express.txt`

> 其实 [] 里面不论有几个字符，他都仅代表某“一个”字符

如果我不想要 oo 前面有 g 的话呢？

`grep -n '[^g]oo' regular_express.txt`

当我们在一组集合字符中，如果该字符组是连续的，例如大写英文/小写英文/数字等等， 就可以使用[a-z],[AZ],[
0-9]等方式来书写，那么如果我们的要求字串是数字与英文呢？

`grep -n '[0-9]' regular_express.txt`

但由于考虑到语系对于编码顺序的影响，因此除了连续编码使用减号“ - ”之外， 你也可以使用如下的方法

`grep -n '[^[:lower:]]oo' regular_express.txt` # 那个 [:lower:] 代表的就是 a-z 的意思！

`grep -n '[[:digit:]]' regular_express.txt`

### 行首与行尾字符 ^ $

如果我想要让 the 只在行首列出呢？ 这个时候就得要使用定位字符了

`grep -n '^the' regular_express.txt`

如果我想要开头是小写字符的那一行就列出呢？

`grep -n '^[a-z]' regular_express.txt`

> 那个 ^ 符号，在字符集合符号（括号[]）之内与之外是不同的！ 在 [] 内代表“反向选择”，在 [] 之外则代表定位在行首的意义！

那如果我想要找出来，行尾结束为小数点 （.） 的那一行

`grep -n '\.$' regular_express.txt`

`cat -An regular_express.txt | head -n 10 | tail -n 6`

在上面的表格中我们可以发现 5~9 行为 Windows 的断行字符 （^M$） ，而正常的 Linux 应该仅有第 10 行显示的那样 （$） 。所以啰，那个 . 自然就不是紧接在 $ 之前喔！也就捉不到 5~9 行了！


因为只有行首跟行尾 （^$），所以，这样就可以找出空白行啦！再来，假设你已经知道在一个程序脚本 （shell script） 或者是配置文件当中，空白行与开头为 # 的那一行是注解，因此如果你要将数据列出给别人参考时，可以将这些数据省略掉以节省保贵的纸张，那么你可以怎么作呢？ 我们以 /etc/rsyslog.conf 这个文件来作范例

`grep -v '^$' /etc/rsyslog.conf | grep -v '^#'`

### 任意一个字符 . 与重复字符 *

在第十章 bash 当中，我们知道万用字符 * 可以用来代表任意（0或多个）字符，但是正则表达式并不是万用字符，两者之间是不相同的！

这两个符号在正则表达式的意义如下：
- . （小数点）：代表“一定有一个任意字符”的意思；
- * （星星号）：代表“重复前一个字符， 0 到无穷多次”的意思，为组合形态

假设我需要找出 g??d 的字串，亦即共有四个字符， 起头是 g 而结束是 d 

`grep -n 'g..d' regular_express.txt`

* 代表的是“重复 0 个或多个前面的 RE 字符”的意义

如果我想要列出有 oo, ooo, oooo 等等的数据， 也就是说，至少要有两个（含） o 以上，该如何是好？是 o* 还是 oo* 还是 ooo* 呢？

`grep -n 'o*' regular_express.txt`


### 限定连续 RE 字符范围 {}
我们可以利用 . 与 RE 字符及 * 来设置 0 个到无限多个重复字符， 那如果我想要限制一个范围区间内的重复字符数呢？

我想要找出两个到五个 o 的连续字串，该如何作？这时候就得要使用到限定范围的字符 {} 了。 但因为 { 与 } 的符号在 shell 是有特殊意义的，因此， 我们必须要使用字符 \ 来让他失去特殊意义才行。

`grep -n 'o\{2\}' regular_express.txt`

假设我们要找出 g 后面接 2 到 5 个 o ，然后再接一个 g 的字串，

`grep -n 'go\{2,5\}g' regular_express.txt`