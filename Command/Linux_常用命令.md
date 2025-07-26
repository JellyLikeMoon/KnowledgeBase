- [set](#set)
- [unset](#unset)
- [grep](#grep)
- [sed](#sed)
- [awk](#awk)
- [split](#split)
- [wc](#wc)
- [uniq](#uniq)
- [join](#join)
- [sort](#sort)
- [cut](#cut)
- [tr](#tr)
- [expr](#expr)
- [spell](#spell)
- [let](#let)
- [find](#find)
- [dd](#dd)
- [fdisk](#fdisk)
- [mkfs](#mkfs)
- [sync](#sync)
- [mount](#mount)
- [du](#du)
- [df](#df)
- [tcpdump](#tcpdump)
- [traceroute](#traceroute)
- [ping](#ping)
- [netstat](#netstat)
- [date](#date)
- [sleep](#sleep)
- [kill](#kill)
- [killall](#killall)
- [pkill](#pkill)
- [ps](#ps)
- [whois](#whois)
- [whoami](#whoami)
- [free](#free)
- [su](#su)
- [sudo](#sudo)
- [id](#id)
- [login](#login)
- [logout](#logout)
- [w](#w)
- [alias](#alias)
- [clear](#clear)
- [dmesg](#dmesg)
- [time](#time)
- [hwclock](#hwclock)
- [crontab](#crontab)
- [zip](#zip)
- [tar](#tar)
- [gzip](#gzip)
- [xargs](#xargs)
- [bc](#bc)
- [tail](#tail)
- [head](#head)
- [getfacl](#getfacl)
- [setfacl](#setfacl)

# set

配置环境变量及控制脚本行为

- 参数

```bash
-a                      # 将后续定义的变量或函数标记为导出
-b                      # 后台程序终止时提醒用户
-C                      # 转向所产生的文件无法覆盖已存在的文件
-d                      # Shell预设会用杂凑表记忆使用过的指令，以加速指令的执行。使用-d参数可取消
-e                      # 若命令返回值不为零, 则终止执行, 但不适用管道命令
-f                      # 取消使用通配符
-h                      # 定义函数时, 定位并保存函数命令, 默认启用状态
-H shell                # 可利用"!"加<指令编号>的方式来执行history中记录的指令
-k                      # 将所有参数当作变量赋值
-l                      # 记录for循环的变量名称
-m                      # 任务完成时显示消息
-n                      # 只读取指令，而不实际执行
-p                      # 启动优先顺序模式
-P                      # 启动-P参数后，执行指令时，会以实际的文件或目录来取代符号连接
-t                      # 执行完随后的指令，即退出shell
-u                      # 如变量不存在则报错, 默认为忽略
-v                      # 打印设立了输入行
-x                      # 打印命令及命令输出结果, 默认为不打印命令
+<参数>                 # 取消某个set曾启动的参数。
```

- 示例

```bash
set -u                  # 设置为变量不存在则报错
set +u                  # 设置为变量不存在则忽略
set -x                  # 设置为打印命令及命令输出结果
set +x                  # 设置为打印命令输出结果但不打印命令
set -e                  # 设置为命令返回值不为零则终止执行
set +e                  # 设置为命令返回值不为零则继续执行
set -eo pipefail        # 任意一个子命令执行失败, 则终止执行

```

# unset

删除指定 shell 变量和函数

- 参数

```bash
-f                      # 仅删除函数
-v                      # 仅删除变量, 不包括只读变量
-n                      # 删除中具有引用属性的变量, 如存在
```

- 示例

```bash
# 删除变量。
declare paper_size='B5'
unset -v paper_size

# 删除函数。
function show_result(){ echo 'Last Command Return: $?'; }
unset -f show_result

# 当不指定选项时，优先删除变量，如果失败则删除函数。
declare -i aa=100
function aa(){ echo 'aa'; }
unset aa
# 变量'aa'已被删除。
declare -p aa
# 函数'aa'存在。
declare -F|grep aa

# 演示unset使用-n选项，name指定了引用变量时的情况。
declare a=3
# 定义引用变量
declare -n b=a
# 查看属性，显示declare -n b="a"
declare -p b
# 显示3
echo ${b}
# 显示a
echo ${!b}
# 指定-n选项时
unset -n b
# 引用变量b已被删除
declare -p b
# 被引用的变量a未被删除
declare -p a

# 演示unset不使用-n选项，name指定了引用变量时的情况。
declare a=3
# 定义引用变量
declare -n b=a
# 查看属性，显示declare -n b="a"
declare -p b
# 显示3
echo ${b}
# 显示a
echo ${!b}
# 不指定-n选项时
unset b
# 引用变量b未被删除，显示declare -n b="a"
declare -p b
# 被引用的变量a被删除
declare -p a
```

# grep

grep 命令用于查找文件里符合条件的字符串或正则表达式。

```bash
grep -l "root" /etc/passwd /etc/shadow              # 仅显示包含匹配字符的文件名
grep -A 2 "root" /etc/passwd /etc/shadow            # 查找文件中包含"root"的行且包含该行下两行
grep -B 2 "root" /etc/passwd /etc/shadow            # 查找文件中包含"root"的行且包含该行上两行
grep -C 2 "root" /etc/passwd /etc/shadow            # 查找文件中包含"root"的行且包含该行上下两行
grep -L "root" /etc/passwd /etc/shadow              # 仅显示不包含匹配字符的文件名
grep -n "root" /etc/passwd                          # 显示行号
grep -v "root" /etc/passwd                          # 反选,不包含"root"的行
grep -r "root" /etc                                 # 在文件夹和子文件夹的文件中递归搜索"root"
grep -i "root" /etc                                 # 忽略大小写
grep -w "root" /etc                                 # 仅匹配 单词的行
grep -c "root" /etc                                 # 统计包含"root"的行数
grep -x "root" /etc                                 # 仅匹配行
grep -e "root" -e "nobody" /etc                     # 搜索多个关键字
grep -f a.txt /root                                 # 从文件中获取参数搜索
grep -m 2 "root" /etc/passwd                        # 最大寻找数
grep -E "^root" /etc/passwd                         # 使用扩展正则表达式(ERE), 默认为BRE
grep -o 'ip'                                        # 只输出匹配的部分
grep -F 'ip'                                        # 搜索固定字符,忽略正则表达式元字符
grep --exclude-dir=filedir                          # 递归搜索时跳过匹配的目录
grep --exclude=filename                             # 跳过匹配的文件
grep --include=filename                             # 只搜索名称匹配的文件
```

# sed

- 参数

```bash
a                                                   # 新增, 当前行的下一行
c                                                   # 取代行的内容
d                                                   # 删除, 删除整行
i                                                   # 插入, 当前行的上一行
s                                                   # 取代指定内容, 正则表达式
p                                                   # 打印, 将选择的数据打印出来
//                                                  # 匹配
/^/                                                 # 行首
/$/                                                 # 行尾
-e                                                  # 多次执行
-i                                                  # 保存
-n                                                  # 执行脚本内容
```

- 示例

```bash
sed -i s/#PermitRootLogin/PermitRootLogin/ /etc/ssh/sshd_config
sed -i '$a # asdadad' /etc/ssh/sshd_config          # 在文件结尾插入行
sed '1,3a\newline' /etc/passwd                      # 指定1-3行插入内容
sed '1,3c\newline' /etc/passwd                      # 指定1-3行替换内容
sed '/linux/c\newline' /etc/passwd                  # 匹配'linux', 并替换包含'linux'的行为'newline'
sed '/linux/cnewline' /etc/passwd                   # 匹配'linux', 并替换包含'linux'的行为'newline'
sed '/linux/c newline' /etc/passwd                  # 匹配'linux', 并替换包含'linux'的行为'newline'
sed '1,2d' /etc/passwd                              # 指定1-2行删除内容
sed '1,$d' /etc/passwd                              # 指定1-最后行删除内容
sed '/admin/d' /etc/passwd                          # 匹配'admin'并删除行
sed -n '5,7p' /etc/passwd                           # 仅列出文件的第5-7行
sed -n '/admin/{s/admin/root/;p;q}' /etc/passwd     # 匹配'admin', 并执行'{}'的内容, 替换admin为root, 打印行, 'q'退出
sed '/admin/p' /etc/passwd                          # 匹配'admin'并打印该行
sed '/linux/,/Linux/i\newline' /etc/passwd          # 多关键词匹配, 插入行
sed 's/linux/newLinux/g' /etc/passwd                # 'g'表示全局查找, 将所有符合条件的替换
sed 's/linux/newLinux/3g' /etc/passwd               # 'g'表示全局查找, 将所有符合条件的前三个替换
sed 's/\.*$/\!/g' /etc/passwd                       # 将所有结尾的'.'替换为'!'

```

# awk

处理文本的脚本语言

- 参数

```bash
# options: 控制awk行为
# pattern: 匹配输入数据模式, 默认对所有行进行操作
# {action}: 匹配的行上执行的动作, 默认为打印整行
awk options 'pattern {action}' file

-F                                                  # 指定输入字段的分隔符
-v variable=value                                   # 设置awk内部变量值, 将外部值传递到awk脚本中的变量
-f <script>                                         # 指定包含awk脚本的文件
```

- 运算符

| 符号                       | 描述                             |
| :------------------------- | :------------------------------- |
| = += -= \*= /= %= ^= \*\*= | 赋值                             |
| ?:                         | C 条件表达式                     |
| \|\|                       | 逻辑或                           |
| &&                         | 逻辑与                           |
| ~ 和 !~                    | 匹配正则表达式和不匹配正则表达式 |
| < <= > >= != ==            | 关系运算符                       |
| 空格                       | 连接                             |
| +-                         | 加，减                           |
| \* / %                     | 乘，除与求余                     |
| +-!                        | 一元加，减和逻辑非               |
| ^ \*\*\*                   | 求幂                             |
| ++ --                      | 增加或减少，作为前缀或后缀       |
| $                          | 字段引用                         |
| in                         | 数组成员                         |

- 内建变量

| 变量名      | 描述                                               |
| :---------- | :------------------------------------------------- |
| &n          | 当前记录的第 n 个字段，字段间由 FS 分隔            |
| $0          | 完整的输入记录                                     |
| ARGC        | 命令行参数的数目                                   |
| ARGIND      | 命令行中当前文件的位置(从 0 开始算)                |
| ARGV        | 包含命令行参数的数组                               |
| CONVFMT     | 数字转换格式(默认值为%.6g)ENVIRON 环境变量关联数组 |
| ERRNO       | 最后一个系统错误的描述                             |
| FIELDWIDTHS | 字段宽度列表(用空格键分隔)                         |
| FILENAME    | 当前文件名                                         |
| FNR         | 各文件分别计数的行号                               |
| FS          | 字段分隔符(默认是任何空格)                         |
| IGNORECASE  | 如果为真，则进行忽略大小写的匹配                   |
| NF          | 一条记录的字段的数目                               |
| NR          | 已经读出的记录数，就是行号，从 1 开始              |
| OFMT        | 数字的输出格式(默认值是%.6g)                       |
| OFS         | 输出字段分隔符，默认值与输入字段分隔符一致。       |
| ORS         | 输出记录分隔符(默认值是一个换行符)                 |
| RLENGTH     | 由 match 函数所匹配的字符串的长度                  |
| RS          | 记录分隔符(默认是一个换行符)                       |
| RSTART      | 由 match 函数所匹配的字符串的第一个位置            |
| SUBSEP      | 数组下标分隔符(默认值是/034)                       |

- 示例

```bash
# 打印整行
awk '{print}' file

# 打印指定列
awk '{print $1, $2}' file

# 使用分隔符指定列
awk -F ',' '{print $1, $2}' file

# 打印行数
awk '{print NR, $0}' file

# 打印满足匹配条件的行
awk '/pattern/ {print NR, $0}' file

# 计算列的总和
awk '{sum += $1} END {print sum}' file

# 打印最大值
awk 'max < $1 {max = $1} END {print max}' file

# 格式化输出
awk '{printf "%-10s %-10s\n", $1, $2}' file

# 使用多个分隔符, 按照分隔符顺序进行分割, 先使用空格分割, 再使用","分割
awk -F '[ ,]'  '{print $1,$2,$5}'   log.txt

# 设置变量
awk -va=1 '{print $1,$1+a}' log.txt

# 过滤第一列大于2的行
awk '$1>2' file

# 过滤第一列等于2的行
awk '$1==2 {print $1,$3}' file

# 过滤第一列大于第二列并且等于"Are"的行
awk '$>2 && $=="Are" {print $1,$2,$3}' file

# 格式化输出
awk 'BEGIN{printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n","FILENAME","ARGC","FNR","FS","NF","NR","OFS","ORS","RS";printf "---------------------------------------------\n"} {printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n",FILENAME,ARGC,FNR,FS,NF,NR,OFS,ORS,RS}'  log.txt

awk -F\' 'BEGIN{printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n","FILENAME","ARGC","FNR","FS","NF","NR","OFS","ORS","RS";printf "---------------------------------------------\n"} {printf "%4s %4s %4s %4s %4s %4s %4s %4s %4s\n",FILENAME,ARGC,FNR,FS,NF,NR,OFS,ORS,RS}'  log.txt

# 输出序号NR, 匹配文本行号
awk '{print NR,FNR,$1,$2,$3}' log.txt

# 指定输出分割符
awk '{print $1,$2,$5}' OFS=" $ "  log.txt

# 正则匹配, "~"表示模式开始, "//"中的内容表示匹配的内容, 输出第二列包含"th"
awk '$2 ~ /th/ {print $2,$4}' log.txt

# 输出包含"/re/"的行
awk '/re/ ' log.txt

# 忽略大小写匹配
awk 'BEGIN{IGNORECASE=1} /this/' log.txt

# 输出不包含"/th/"的行
awk '!/th/ {print $2,$4}' log.txt

```

- 注意

> 关于 awk 脚本，我们需要注意两个关键词 BEGIN 和 END。
>
> - BEGIN{ 这里面放的是执行前的语句 }
> - END {这里面放的是处理完所有的行后> 要执行的语句 }
> - {这里面放的是处理每一行时要执行的语句}

- 脚本示例

```bash
#!/bin/awk -f
# 运行前
BEGIN {
    math = 0
    english = 0
    computer = 0

    printf "NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL\n"
    printf "---------------------------------------------\n"
}
# 运行中
{
    math+=$3
    english+=$4
    computer+=$5
    printf "%-6s %-6s %4d %8d %8d %8d\n", $1, $2, $3,$4,$5, $3+$4+$5
}
# 运行后
END {
    printf "---------------------------------------------\n"
    printf "  TOTAL:%10d %8d %8d \n", math, english, computer
    printf "AVERAGE:%10.2f %8.2f %8.2f\n", math/NR, english/NR, computer/NR
}

```

# split

split 命令用于将一个文件分割成数个, 默认情况下将按照每 1000 行切割成一个文件

```bash
split -1000 test.txt test.txt                       # "-1000"按行数分割, 设置切割后的前置文件名split会自动加编号
split -b 20  test.txt                               # "-b 20"按照字节分割
split -C 20  test.txt                               # "-C 20"按照直接分割,但是尽量维持每行的完整性
```

# wc

wc 命令用于计算字数, 利用 wc 指令我们可以计算文件的 Byte 数、字数、或是列数,若不指定文件名称、或是所给予的文件名为"-", 则 wc 指令会从标准输入设备读取数据

```bash
wc -l test.txt                                      # 显示行数
wc -w test.txt                                      # 显示单词数
wc -c test.txt                                      # 显示字节数
```

# uniq

uniq 命令用于检查及删除文本文件中重复出现的行列,一般与 sort 命令结合使用

```bash
uniq -c test.txt                                    # 删除重复行并统计每行出现的次数
uniq -d test.txt                                    # 仅显示重复出现的行
uniq -f test.txt                                    # 忽略比较指定的栏位
uniq -s test.txt                                    # 忽略比较指定的字符
uniq -u test.txt                                    # 仅显示出一次的行列
uniq -w test.txt                                    # 指定要比较的字符
```

- 注意
  > 当重复的行并不相邻时,uniq 命令是不起作用的, 需配合 sort 命令使用

# join

_将两个文件中指定列内容相同的行连接起来,输出到标准输出_  
_使用注意:连接的文件内容须按照字母或者数字排序_

```bash
-a FILENUM                                          # 显示所有行,FILENUM为"1"或"2",表示为文件1,文件2
-e <EMPTY>                                          # 如两个文件中找不到指定字段(列)的行,则输出该"<>"字符串替代
-i, --ignore-case                                   # 忽略大小写差异
-j FIELD                                            # equivalent to '-1 FIELD -2 FIELD'
-o 1.1                                              # 按照指定文件的字段(列)显示结果,"1.1"表示第一个文件第一个字段(列)
-t <CHAR>                                           # 使用<CHAR>作为字段(列)的分隔符号, 输出符合该分隔符的行
-v FILENUM                                          # 与"-a"类似,仅显示不同行
-1 FIELD                                            # 指定文件1用于连接的字段,即相同的列的字段
-2 FIELD                                            # 指定文件2用于连接的字段
--check-order                                       # 检查输入是否正确排序,即是否所有行都是可匹配的
--nocheck-order                                     # 不检查输入是否正确排序
--header                                            # 将每个文件的第一行视为字段头,打印时不匹配
-z, --zero-terminated                               # 行分隔符是NUL,而不是换行符
--help                                              # 显示帮助
--version                                           # 显示版本信息
```

- 示例

```bash
root@ubtest:~/shelltest# cat 1.txt
1 limao master
2 limao master
3 limao master
4 limao master
5 limao master
6 limao master
7 limao master
8 limao master
root@ubtest:~/shelltest# cat 2.txt
1 huawei master
2 huawei master
3 huawei master
4 huawei master
5 huawei master
6 huawei master
7 huawei master
8 huawei master
root@ubtest:~/shelltest# join -o 1.2 2.2 1.txt 2.txt
limao huawei
limao huawei
limao huawei
limao huawei
limao huawei
limao huawei
limao huawei
limao huawei
limao huawei
root@ubtest:~/shelltest# join -1 1  1.txt 2.txt
1 limao master sun
2 limao master sun
3 limao master sun
4 limao master sun
5 limao master sun
root@ubtest:~/shelltest# join -t " "  1.txt 2.txt
1 limao master sun
2 limao master sun
3 limao master sun
4 limao master sun
5 limao master sun
```

# sort

sort 可针对文本文件的内容, 以行为单位, 按照 ASCII 码的次序排序

```bash
sort -r test.txt                                    # 反向排序
sort -u test.txt                                    # 排序同时输出去重的结果
sort -o ./test1.txt test.txt                        # 指定输出文件
sort -b test.txt                                    # 忽略每行前面开始出的空格字符
sort -c test.txt                                    # 检查文件是否已经按照顺序排序
sort -f test.txt                                    # 排序时,将小写字母视为大写字
sort -t , test.txt                                  # 指定排序时所用的字段(列)分隔符号
sort -n test.txt                                    # 依照数值的大小排
sort -h test.txt                                    # 依照数值的大小排, 数值人性化, 展现为K, M, G
sort -k 2 test.txt                                  # 按指定的列进行排序
```

# cut

_用于显示每行从开头算起 num1 至 num2 的字符_

```bash
-b, --bytes=LIST                                    # 以字节为单位进行分割(N-,N-M,-M)
-c, --characters=LIST                               # 以字符为单位进行分割
-d, --delimiter=DELIM                               # 指定字段(列)分隔符,默认为制表符
-f, --fields=LIST                                   # 显示指定字段(列)的内容, 默认分隔符为 "TAB"
-n                                                  # 不分割多字节多字符,仅与"-b"使用
--complement                                        # 除被选择列之外的全部列的内容
-s, --only-delimited                                # 不打印不含分隔符的行
--output-delimiter=STRING                           # 指定"STRING"为输出字段(列)之间的分割符
-z, --zero-terminated                               # 行分隔符为NUL,不是换行符
--help                                              # 显示帮助信息
--version                                           # 显示版本信息
```

- 示例

```bash
cut -f2 -d" " --complement test.txt                 # 以 " " 为分隔符, 显示除第二列之外的所有列,
cut -f2 -d" " test2.txt                             # 以 " " 为分隔符, 显示第二列
cut -b 1 1.txt                                      # 显示第一个字节的列
cut -b 1-4 1.txt                                    # 显示1-4字节的列
cut -b 2- 1.txt                                     # 显示从第二个字节开始的所有列
cut -b -4 1.txt                                     # 显示从第一个字节到第四个字节的列
cut -f 1,2 -d" " --output-delimiter=* 1.txt         # 显示 1 2 列的内容时, 以 "*" 为分隔符
cut -f 1,2 -d" " -s --output-delimiter=* 1.txt      # 显示 1 2 列的内容时, 以 "*" 为分隔符, 且不显示不包含指定分隔符 " " 的行
```

# tr

_用于字符转换,删除,压缩_  
tr [OPTION] SET1 SET2

```bash
-c, -C, --complement                                # 将除了SET1中的字符, 其余字符用SET2的字符替换
-d, --delete                                        # 删除SET1中所有字符
-s, --squeeze-repeats                               # 用SET1中指定的字符替换对应的重复字符
-t, --truncate-set1                                 # 将SET1中的字符用SET2对应位置的字符替换
--help                                              # 帮助信息
--version                                           # 版本信息
```

- 字符参数

```bash
\NNN            character with octal value NNN (1 to 3 octal digits)
\\              backslash
\a              audible BEL
\b              backspace
\f              form feed
\n              new line
\r              return
\t              horizontal tab
\v              vertical tab
CHAR1-CHAR2     all characters from CHAR1 to CHAR2 in ascending order
[CHAR*]         in SET2, copies of CHAR until length of SET1
[CHAR*REPEAT]   REPEAT copies of CHAR, REPEAT octal if starting with 0
[:alnum:]       all letters and digits
[:alpha:]       all letters
[:blank:]       all horizontal whitespace
[:cntrl:]       all control characters
[:digit:]       all digits
[:graph:]       all printable characters, not including space
[:lower:]       all lower case letters
[:print:]       all printable characters, including space
[:punct:]       all punctuation characters
[:space:]       all horizontal or vertical whitespace
[:upper:]       all upper case letters
[:xdigit:]      all hexadecimal digits
[=CHAR=]        all characters which are equivalent to CHAR

```

- 示例

```bash
echo "hello world" | tr 'a-z' 'A-Z'                                 # 将字符串中 "a-z" 替换为 "A-Z"
echo "hello world" | tr ' ' '-'                                     # 将字符串中 " " 替换为 "-"
echo "hello world123456" | tr -d '0-9'                              # 删除字符串中 "0-9"
echo "hello world" | tr -t 'hel' 'lhe'                              # 按set1和set2中对应位置替换字符
echo "hello worldaaaavvvv" | tr -s '[av\t]'                         # 删除连续重复的 "av\t" 三种字符, 使该字符唯一
echo "hello worldaaaavvvv" | tr -s '[av\t]' 'n'                     # 删除连续重复的 "av\t" 三种字符, 使该字符唯一, 并替换为 "n"
echo aa.,a 1 b#$bb 2 c*/cc 3 ddd 4 | tr -d -c '0-9 \n'              # 删除除了 "0-9 \n"之外的所有字符
echo aa.,a 1 b#$bb 2 c*/cc 3 ddd 4 | tr -c '0-9 \n' 'o'             # 将除 '0-9 \n' 之外的字符替换为 'o'
```

# expr

_计数器, 可求表达式变量的值, 用于整数或字符串_  
_参数之间需要空格间隔, 特殊字符需要反斜杠, 字符串或空格需要引号括起来_

- 参数

```bash
index                           # 显示指定字符在字符串中的位置
length                          # 计算字符串长度
substr                          # 显示指定位置字符串
```

- 示例

```bash
expr length "hello world"
expr index "hello world" o
expr substr "hello world" 3 5
expr 14 % 2
expr 14 \* 2
expr 14 + 2
```

# spell

_拼写检查工具, 更高级的拼写检查功能可以使用 aspell 或 ispell_

- 参数

```bash
-a                                  # 检查单个单词
-b                                  # 使用英国英语字典
-c                                  # 检查一个文件的拼写错误
-n                                  # 打印行号
-o                                  # 打印文件名
-v                                  # 当单词不在字典中时, 打印该单词
-D file                             # 使用指定字典作为ispell的字典
-d file                             # 使用指定文件作为个人字典
```

- 示例

```bash
spell -b file.txt                    # 指定使用英国字典进行拼写检查
spell -c file.txt                    # 检查文件的拼写错误, 并提供交互式修改错误
spell -d dict file.txt               # 指定自定义字典进行拼写检查
spell -n file.txt                    # 显示拼写错误的单词的行号
spell -o file.txt file1.txt          # 显示拼写错误的单词的文件名

```

# let

_内建计算器, 执行算术表达式_

- 参数

| 符号                   | 描述                     |
| :--------------------- | :----------------------- |
| id++, id--             | 变量后增量、变量后减量   |
| ++id, --id             | 变量预增量、变量预减量   |
| -, +                   | 正 号、负号              |
| !, ~                   | 逻辑否、按位取反         |
| \*\*                   | 幂运算                   |
| \*, /, %               | 乘 法、除法、取余        |
| +, -                   | 加法、减法               |
| <<, >>                 | 按位左移、右移           |
| <=, >=, <, >           | 比较                     |
| ==, !=                 | 等于、不等于             |
| &                      | 按位与                   |
| ^                      | 按位异或                 |
| \|                     | 按位或                   |
| &&                     | 逻辑与                   |
| \|\|                   | 逻辑或                   |
| expr ? expr : expr     | 条件运算符（三元运算符） |
| =, \*=, /=, %=, +=, -= | 赋值                     |
| <<=, >>=, &=, ^=, \|=  | 赋值                     |

- 示例

```bash
let "myvar =2" "myvar1=1" "myvar2=myvar1+myvar"; echo $myvar2
let "myvar=2" "myvar2=myvar++" ; echo $myvar $myvar2
let "a=1+2"; echo $a
```

# find

_在指定目录下查找文件和目录_

- 参数

```bash
-name filename                      # 按文件名查找, 支持通配符"*", "?"
-iname filename                     # 按文件名查找, 但忽略大小写, 支持通配符"*", "?"
-type type                          # 按文件类型查找,"f d l b s"等
-size [-+]size[cwbkMG]              # 按文件大小查找, "c"字节, "w"字数, "b"块数, "k"KB, "M"MB
-user username                      # 按文件所有者查找
-nouser username                    # 查找无有效属主的文件
-group groupname                    # 按文件所属组查找
-nogroup groupname                  # 查找无有效属组的文件
-follow                             # 查找链接符号文件时, 追踪链接所指文件位置
-prune filename                     # 忽略某个文件或目录
-path "*app/*"                      # 按路径查找
-or                                 # 或, 满足其中一个条件
-and                                # 与, 必须同时满足条件
-not                                # 否, 不是
-delete                             # 删除查找到的结果, 仅删除文件或空目录
-perm                               # 按文件权限查找
-empty                              # 查找空文件或空目录
-amin                               # 查找n分钟内被访问的文件
-atime                              # 查找n天内被访问的文件
-cmin                               # 查找n分钟内状态发生变化的文件(例如权限等)
-ctime                              # 查找n天内状态发生变化的文件(例如权限等)
-mmin                               # 查找n分钟内被修改过的文件
-mtime                              # 查找n天内被修改过的文件
-exec                               # 执行后续命令
-execdir                            # 在包含匹配文件的目录执行后续命令
-regex                              # 使用正则表达式
-iregex                             # 忽略大小写, 使用正则表达式
-maxdepth 3                         # 限制最大目录搜索层数
-print                              # 打印完整文件名, 后接换行符
-print0                             # 打印完整文件名, 后接空字符
```

- 示例

```bash
# 当前目录下查找后缀为".txt"的文件
find . -name "[A]*.txt"

# 当前目录下查找大小超过1MB的文件
find . -size +1M

# "/home"目录下查找7天内修改过的文件
find /home -mtime +7

# "/var/log"目录下查找类型为"f"的文件, 删除之前需要确认
find /var/log -type f -mtime +7 -ok rm {} \;

# 当前目录下查找名称为"pattern"的文件, 并删除无需确认
find . -name "pattern" -exec rm {} \;

# 当前目录下查找文件权限为664且类型为f的文件, 并列出
find . -type f -perm 644 -exec ls -l {} \;

# 查找名称为"*.js"和"*.html", 但是路径不是"*programs*"的文件
find . \( -name "*.js" -or -name "*.html" \) -and -not -path "*programs*"

# 使用正则表达式匹配文件路径
find . -regex ".*\(\.txt\|\.pdf\)$"

# 忽略指定目录
find . \( -path ./sk -o  -path ./st \) -prune -o -name "*.txt" -print
find . -path "./sk" -prune -o -name "*.txt" -print

```

注意:

> +n: 表示比 n 更早之前的时间  
> -n: 表示 n 天内的时间  
>  n: 表示 n 天当天内的时间

# dd

从标准输入或文件读取数据, 根据指定格式转换数据, 再输出到文件, 设备或标准输出

- 参数

```bash
if=file                     # 输入文件名，默认为标准输入。即指定源文件。
iflag=flags                 #
of=file                     # 输出文件名，默认为标准输出。即指定目的文件。
oflag=flags
ibs=bytes                   # 一次读入bytes个字节，即指定一个块大小为bytes个字节。
obs=bytes                   # 一次输出bytes个字节，即指定一个块大小为bytes个字节。
bs=bytes                    # 同时设置读入/输出的块大小为bytes个字节。
cbs=bytes                   # 一次转换bytes个字节，即指定转换缓冲区大小。
skip=blocks                 # 从输入文件开头跳过blocks个块后再开始复制。
seek=blocks                 # 从输出文件开头跳过blocks个块后再开始复制。
count=blocks                # 仅拷贝blocks个块，块大小等于ibs指定的字节数。
status=level                # 打印错误输出的等级信息,
                            # "none"    只显示错误消息
                            # "noxfer"  隐藏最终的传输统计信息
                            # "progress"显示定期的出传输统计信息
conv=convs                  # 根据关键字进行转换文件
```

- 示例

```bash
# 制作系统启动盘
dd if=boot.img of=/dev/fd0 bs=1440k

# 转换文字, 将源文件中全部大写字母转换为小写字母
dd if=testfile of=testfile1 conv=ucase

# 备份磁盘数据
dd if=/dev/sda of=/dev/sdb

# 测试硬盘读写速度, 写, 读
dd if=/dev/zero bs=1024 count=1000000 of=/root/1Gb.file
dd if=/root/1Gb.file bs=64k | dd of=/dev/null

# 备份数据到文件
dd if=/dev/hda of=/root/image count=1 bs=512

# 修复硬盘
dd if=/dev/sda of=/dev/sda

# 销毁硬盘
dd if=/dev/urandom of=/dev/hda1
```

# fdisk

用于磁盘分区, 可以创建、编辑、删除、显示硬盘分区

- 参数

```bash
fdisk -lu /dev/sda                          # 显示分区表信息
fdisk -x /dev/sda                           # 显示分区表详细信息
fdisk -t                                    # 修改分区的类型
fdisk -u                                    # 以扇区为单位显示分区信息
fdisk -w                                    # 将分区表写入硬盘
blkid /dev/sda                              # 查询磁盘UUID
```

- 示例

```bash
# 创建分区
fdisk /dev/sda

# 永久挂载分区
echo "/dev/sda /ssd xfs default 0 0" >> /etc/fstab

# 使用UUID永久挂载磁盘分区
echo "UUID=1851e23f-1c57-40ab-86bb-5fc5fc606ffa /ssd xfs default 0 0" >> /etc/fstab

# 查看分区信息
mount -a
lsblk
```

# mkfs

建立文件系统

- 参数

```bash
mkfs -V                                     # 显示操作详细信息
mkfs -t                                     # 指定文件系统格式
mkfs -c                                     # 检查指定设备是否损坏
```

- 示例

```bash
# 将硬盘分区格式化为ext3
mkfs -V -t ext3 /dev/sda2

# 将硬盘分区的2G空间格式化为ext3
mkfs -V -t ext3 /dev/sda2 2G
```

# sync

```bash

```

# mount

用于挂载文件系统, 包括但不限于硬盘驱动器, 网络共享, 其他文件系统等

- 选项

```bash
mount -t                                    # 指定挂载的文件类型
mount -l                                    # 显示文件系统的卷标
mount -r                                    # 只读挂载
mount -w                                    # 读写挂载
mount -n                                    # 不更新/etc/mtab
mount -a                                    # 自动挂载所有支持自动挂载的设备(在/etc/fstab中且挂载选项启用自动挂载)
mount -L                                    # 根据卷标指定挂载设备
mount -U                                    # 根据UUID指定挂载设备
mount -B                                    # 绑定目录到另一个目录
mount -o                                    # 挂载文件系统选项, 默认为 "rw, suid, dev, exec, auto, nouser, and async"
    async                                   # 异步模式
    sync                                    # 同步模式
    atime/noatime                           # 包含目录和文件
    diratime/nodiratime                     # 目录的访问时间戳
    auto/noauto                             # 是否自动挂载
    exec/noexec                             # 是否支持将文件系统上的应用程序运行为进程
    dev/nodev                               # 是否支持在此文件系统上使用设备文件
    suid/nosuid                             # 是否支持在此文件系统上使用特殊权限
    remount                                 # 重新挂载
    ro                                      # 只读
    rw                                      # 读写
    user/nouser                             # 是否允许普通用户挂载此设备
    acl                                     # 启用文件系统的acl功能
```

- 示例

```bash
# 简单挂载
mount /dev/sdb1 /mnt

# 卸载文件系统
umount /mnt

# 指定挂载文件系统类型
mount -t ext4 /dev/sdb1 /mnt

# 挂载网络文件系统
mount -t nfs server:/share /mnt

# 指定挂载文件访问权限
mount -o ro /dev/sdb1 /mnt
mount -o rw,user /dev/sdb1 /mnt

```

`/etc/fstab` 文件说明

|                 挂载的设备或文件                 | 挂载点     | 文件系统类型 | 挂载选项 | 转储频率                             |       自检次序       |
| :----------------------------------------------: | ---------- | ------------ | -------- | ------------------------------------ | :------------------: |
| UUID=, 设备文件, LABEL=, 伪文件系统(proc, sysfs) | 指定文件夹 | ext3         | defaults | 0:不备份, 1:每天转储, 2:每隔一天转储 | 0:不自检, 1:首次自检 |

# du

查看目录或文件的大小, 默认仅显示目录大小

```bash
du -sh                                      # 当前目录总大小
du -sh <目录|文件>                           # 当前目录/文件总大小
du -ah                                      # 目录及文件的大小
du -h --max-depth=1 /path                   # 最大搜索目录层级为1
```

# df

查看磁盘分区空间大小

```bash
df -h                                       # 以人类可读的方式为单位显示大小, 如k, m, g
df -T                                       # 显示文件系统类型
df -a                                       # 显示所有文件系统, 包括伪文件系统, 重复文件系统, 不可访问的文件系统
df -H                                       # 以1000为单位显示大小
df -i                                       # 列出inode信息, 而非块儿使用情况
df -k                                       # 等同于 "--block-size=1k"
df -t                                       # 列出指定文件类型的文件系统
df -l                                       # 仅显示本地文件系统
df -x                                       # 排除指定类型的文件系统
```

# tcpdump

```bash

```

# traceroute

```bash

```

# ping

```bash

```

# netstat

```bash

```

# date

```bash

```

# sleep

延迟指定时间

```bash
sleep 1s                            # 暂停1秒
sleep 1m                            # 暂停1分钟
sleep 1h                            # 暂停1小时
sleep 1d                            # 暂停1天
```

# kill

向进程发送信号, 默认为发送终止进程指令

```bash
kill -l                             # 列出可用的信号
kill -l SIGHUB                      # 根据信号名称显示信号编号
kill -l 1                           # 根据信号编号显示信号名称
kill -s SIGNAME PID                 # 根据信号名称对进程发送信号
kill -n SIGID PID                   # 根据信号编号对进程发送信号
kill -9 PID                         # 强制彻底终止进程, 但不会进行后续进程清理工作
kill -15 PID                        # 安全的终止进程
kill -15 PID1 PID2 ...              # 同时终止多个进程
kill PID                            # 默认选项, 同 "-15"
kill -1 PID                         # 重新加载进程
```

# killall

终止指定名称的所有进程

```bash
killall -9 mysql                    # 强制彻底终止所有名称为mysql的进程
killall -15 mysql                   # 安全的终止所有名称为mysql的进程
killall -l                          # 列出所有可用信号
killall -e                          # 对长名称的进程进行精确匹配
killall -I                          # 匹配进程名称时不区分大小写
killall -g                          # 终止进程组
killall -i                          # 终止进程前询问用户确认
killall -q                          # 不显示错误或告警信息
killall -s                          # 发送指定信号, 默认为 "SIGTEAM"
killall -u                          # 终止指定用户运行的进程
killall -v                          # 显示信号是否发送成功的详细信息
killall -w                          # 等待进程终止后再退出
killall -n                          # 匹配与指定进程PID属于同一命名空间的进程
```

# pkill

终止匹配进程名称的一类进程

```bash
pkill -9 -t pts/1                   # 强制终止隶属于虚拟终端 "pts/1" 的进程
pkill -u username                   # 终止指定用户的所有进程
pkill mysql                         # 终止名为 "mysql" 的所有进程
pkill --signal SIGKIll mysql        # 更改发送指令
```

# ps

```bash

```

# whois

```bash

```

# whoami

```bash

```

# free

```bash

```

# su

```bash

```

# sudo

```bash

```

# id

```bash

```

# login

```bash

```

# logout

```bash

```

# w

```bash

```

# alias

```bash

```

# clear

```bash

```

# dmesg

```bash

```

# time

```bash

```

# hwclock

```bash

```

# crontab

```bash

```

# zip

```bash

```

# tar

_文件或目录打包解压工具_

- 参数

```bash
-c                                      # 这会创建一个存档文件.
-x                                      # 该选项提取了存档文件.
-f                                      # 指定归档文件的文件名.
-v                                      # 这将打印终端上任何tar操作的粗略信息.
-t                                      # 仅列出一个归档文件内的所有文件,不解压
-u                                      # 归档一个文件, 然后把它添加到一个现有的归档文件中.
-r                                      # 追加文件至归档文件中
-z                                      # 使用gzip压缩创建一个tar文件
-j                                      # 使用bzip2压缩法创建一个存档文件
-W                                      # 选项验证一个归档文件.
-C                                      # 指定解压目录
--delete                                # 删除归档文件中的文件
```

- 示例

```bash
# 使用gzip格式压缩
tar -czvf sales.tar.gz  sales1.pdf sales2.pdf sales3.pdf
# 解压
tar -xzvf sales.tar.gz -C ./
```


# gzip

```bash

```

# xargs

```bash

```

# bc

```bash

```

# tail

```bash

```

# head

```bash

```

# getfacl

查看 ACL 权限

访问控制列表(ACL, Access Contrl List), 针对文件/目录的访问控制列表, 是 UNIX 文件权限管理的补充, 可以给任何用户/用户组设置任何文件/目录的访问权限

```bash
getfacl <file|directory>                    # 查看文件和目录的ACl权限
```

# setfacl

设置 ACL 权限

- 目录具有 default 权限, 该权限会自动应用于该目录下所有的新建文件和子目录.
- 子目录会继承父目录的 default 权限, 并且自身的权限也继承自父目录的 default 权限.
- 文件无法继承 default 权限中的执行权限, 文件的权限为 666

```bash
setfacl -m -R u:user:--- test.txt           # 设置用户ACL权限, 递归该目录下所有文件和目录生效
setfacl -m g:user:rwx test.txt              # 设置用户组ACL权限
setfacl -x u:user test.txt                  # 删除ACL权限
setfacl -b test.txt                         # 清除所有ACl权限
setfacl -d -m u::--- test.txt               # 设置目录的ACL default权限
setfacl -d -m g::--- test.txt               # 设置目录的ACL default权限
setfacl -m d:u:user:--- test.txt            # 修改目录ACL default权限
setfacl -k test.txt                         # 移除ACL default权限, 仅对目录生效
```
