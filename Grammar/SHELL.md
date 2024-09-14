- [Shell 语法篇](#shell-语法篇)
- [变量](#变量)
- [字符串操作](#字符串操作)
- [数组](#数组)
- [运算符](#运算符)
- [printf](#printf)
- [脚本参数](#脚本参数)
- [if 判断](#if-判断)
- [case 判断](#case-判断)
- [for 循环](#for-循环)
- [while 循环](#while-循环)
- [until 循环](#until-循环)
- [函数](#函数)
- [注释](#注释)
- [环境变量](#环境变量)
- [运行 shell](#运行-shell)
- [Shell 命令篇](#shell-命令篇)
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
- [look](#look)
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
- [procinfo](#procinfo)
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
- [unset](#unset)
- [hwclock](#hwclock)
- [set](#set)
- [crontab](#crontab)
- [zip](#zip)
- [tar](#tar)
- [gzip](#gzip)
- [xargs](#xargs)
- [bc](#bc)
- [tail](#tail)
- [head](#head)
- [pkill](#pkill)

---

# Shell 语法篇

---

# 变量

* 变量赋值

```bash
name='name name'                 # '' 定义变量,完全按照字面意思处理,不解析变量或执行命令
name="name name"                 # "" 定义变量可解析变量,执行命令,"=" 两侧不能有空格,如变量中有空格需要 ""或 '' 包含
name=123
```

* 只读变量

```bash
name="name"
readonly name                    # 只读变量:一旦写入不能改变值,输出只读变量可以不加$
echo name
```

- 全局变量
```bash
export var

```

* 从键盘读取变量
```bash
read name                        # 读取输入的变量
read -p "hello " name            # 显示提示信息
read -t 5 -p "5s" name           # "-t" 等待时间,等待时间内不输入则退出
read -n 2 name                   # "-n" 限制输入的字符数

```

* 取消变量

```bash
unset name                      # 取消变量
```

* 输出变量

```bash
echo $name
echo ${name}                    # "{}" 变量界定符
```

* 命令替换

```bash
tm=$(data)                      # 命令替换(command substitutions): 将命令data的返回值作为变量tm的值
tm=`data`                       # 旧方法
```

# 字符串操作

* 字符串拼接

```bash
str1="book"
str2="hand"
str3=$str1$str2                 # 字符拼接
```

* 字符串大小写转换

```bash
echo ${distro^^}                # 小写转大写
echo ${distro,,}                # 大写转小写
echo ${distro^}                 # 仅首字母大写
echo ${distro,}                 # 仅首字母小写
echo ${distro^^[jn]}            # 指定字母大写
expr index "$str" "word"        # 查找特定单词或字母的索引值$str

```

* 读取字符串值

```bash
echo ${var1:-word}              # 如var为空或"unset var"则返回"word",否则返回var的值,不改变var的值
echo ${var2:+word}              # 如var被定义,则返回"word",否则返回空, 不改变var的值
echo ${var1:=word}              # 如var为空或"unset var"则返回"word",否则返回var的值,并修改var的值为"word"
echo ${var1:?word}              # 如var为空或被删除,将"word"传送到标准错误输出,检测变量是否被正常赋值
echo ${!v*}                     # 通过前缀为 "V" 的变量,并列出满足条件的变量
```

* 字符串长度

```bash
echo ${#name}                   # 变量的字符长度
```

* 字符串替换

```bash
echo ${distro/free/more}        # 将第一个匹配的free替换为more
echo ${distro//free/more}       # 将所有匹配的free替换为more
echo ${distro/#free/more}       # 匹配以"free"开头的字符串并将"free"替换为"more"
echo ${distro/%free/more}       # 匹配以"free"结尾的字符串并将"free"替换为"more"
```

* 字符串截取

```bash
url=www.sina.com.cn
echo ${url#*.}                  # 从前往后删,直到第一个 "."(包含) 之前的内容,最短匹配
echo ${url##*.}                 # 从前往后删,直到最后一个 "."(包含)之前的内容,最长匹配
echo ${url%.*}                  # 从后往前,最短匹配,直到第一个 "."(包含)之后的内容
echo ${url%%.*}                 # 从后往前,最长匹配,直到最后一个 "."(包含)之后的内容

echo ${distro/free}             # 删除free
echo ${distro//-}               # 删除所有破折号,需要双斜杠
echo ${distro:0:6}              # 字符串提取,0表示起始位置,6表示从起始位置开始提取6个字符
echo ${distro:2}                # 字符串提取,从索引2位置开始提取余下所有的字符
echo ${distro:(-2)}             # 截取倒数2位字符
echo ${distro:(-2):5}           # 截取倒数2-5位的字符
```

# 数组

* 定义数组

```bash
array_name=(1 2 3)              # 一次性定义数组,使用空格间隔数组元素

array_name[0]=1                 # 单独定义数组,也是数组赋值
array_name[1]=1
array_name[2]=1
```

* 数组方法

```bash
array_name+=("2")               # 追加数组元素
echo ${array_name[0]}           # 读取数组指定元素
echo ${array_name[-3]}          # 读取数组倒数第三位元素
unset array_name[0]             # 删除数组元素
unset array_name                # 删除数组
echo ${#array_name[*]}          # 获取数组长度
echo ${#array_name[@]}          # 获取数组长度
echo ${array_name[@]}           # 获取数组中的所有元素以空格间隔
echo ${array_name[*]}           # 获取数组中的所有元素一整个字符串
echo ${#array_name[n]}          # 获取单个元素长度
echo ${array_name[@]:2:5}       # 元素截取,截取2-5位的元素
echo ${array_name[@]/1/2}       # 元素替换,将元素 "1" 替换为元素 "2"
echo ${!array_name}             # "!" 获取数组所有键
echo ${!array_name[@]}          # "!" 获取数组下标
```

* 关联数组

```bash
declare -A array_name=(["googole"]="1234" ["googole"]="1234")   # 关联数组,元素之间以 " " 间隔
```

# 运算符

- 算数运算符

*仅用于整数运算*
```bash
expr $a + $b                    # 加法
expr $a - $b                    # 减法
expr $a \* $b                   # 乘法
expr $a / $b                    # 除法
expr $a % $b                    # 取余
```

```bash
echo $(($n1+$n2))               # 加法
echo $(($n1-$n2))               # 减法
echo $(($n1*$n2))               # 乘法
echo $(($n1**$n2))              # 幂运算
echo $(($n1/$n2))               # 除法
echo $(($n1%$n2))               # 取余
echo $(($n1++))                 # 后置自增
echo $(($n1--))                 # 后置自减

```

```bash
echo $[$n1+$n2]                 # 使用方法同上$(())

```

```bash
val=$(echo "scale=2;3.14/2" | bc)  # "bc" 小数运算,"scale=2" 表示精度为2
echo $val
```

```bash
let sum=2+3;echo $sum

let i++;echo $i     i++表示i=i+1

#------------------let方法运算---------------
ip=127.0.0.1
i=1
while [ $i -le 5 ]
do
        ping -c1  $ip &>/dev/null
        if [ $? -eq 0 ];then
                echo "$ip is up..."
        fi
        let i++
done

```

- 关系运算符

*使用在 "[]" 中*
```bash
-eq                             # 等于为真 =
-ne                             # 不等于为真 !=
-gt                             # 大于为真 >
-ge                             # 大于等于为真 >=
-lt                             # 小于为真 <
-le                             # 小于等于为真 <=
==                              # 全等
!=                              # 不等
```

- 逻辑运算符

*"[[]]" "[]" 结果为布尔值*  
*"[[]]" 内可使用 "&& ||" 等逻辑运算符,内可使用 "< > == = != =~"*  
*"[]"内不能使用"&& || < >",可使用"! -o -a"*

```bash
[ !false ]                      # 非运算,表达式为True则返回False
[ -o ]                          # 或运算,有一个表达式为True则返回True
[ -a ]                          # 与运算,两个都为True则返回True

```

```bash
;                               # 所有命令依次执行
&&                              # 逻辑与,前面执行成功,后面才执行
||                              # 逻辑或,前面执行失败后面才执行
```

- 字符运算符

```bash
[ -Z $a ]                       # 字符串为0则返回True
[ -n $a ]                       # 字符串不为0则返回False
[ $a ]                          # 字符串不为空则返回True
[ $a == $b ]                    # 相等
[ $a = $b ]                     # 相等
[ $a != $b ]                    # 不相等
[ $a =~ $b ]                    # 匹配 "$b" 的内容, "$b" 为正则表达式
```

- 文件测试运算符

```bash
[ -b file ]                     # 检测文件为块设备则True
[ -c file ]                     # 检测文件为字符设备则True
[ -d file ]                     # 检测文件为目录则True
[ -f file ]                     # 检测文件为普通文件则True
[ -g file ]                     # 检测文件设置SGID位则True
[ -k file ]                     # 检测文件设置粘着位则True
[ -p file ]                     # 检测文件是有名管道则True
[ -u file ]                     # 检测文件设置SUID位则True
[ -r file ]                     # 检测文件可读则True
[ -w file ]                     # 检测文件可写则True
[ -x file ]                     # 检测文件可执行则True
[ -s file ]                     # 检测文件为空则True
[ -e file ]                     # 检测目录存在则True
```

- 条件测试符

```bash
test $[a] -eq $[b]
test $str1==$str2
test -e /bin/sh
```

```bash
[]
[[]]

```

- 通配符

```bash
?                               # 单个任意字符
*                               # 匹配0到任意个 
[]                              # 匹配任意一个
[A-Z]                           # 取范围
[^]                             # 取反
^                               # 行首限定符
$                               # 行尾限定符

root@devtest:~# cat /etc/ssh/sshd_config | grep -n "Use[a-z]"
root@devtest:~# cat /etc/ssh/sshd_config | grep -n "Use*"
root@devtest:~# cat /etc/ssh/sshd_config | grep -n "Use[^a-z]"
```

重定向符
```bash
>                               # 标准输出:"A > B" A的输出写入到B中
>>                              # 标准输出追加:"A >> B" A的输出追加到B中
2>                              # 标准错误输出:"A >> B" A的错误输出追加到B中
2>>                             # 标准错误输出追加:"A >> B" A的错误输出追加到B中
&>>                             # 重定向标准输出和标准错误输出
<                               # 标准输入:
<<END                           # 标准输入追加: "END" 为自定义结束符
2>&1                            # 标准错误输出转到标准输出
|                               # "A | B" A的输出是B的输入

```

# printf

- 格式化参数
*用法: printf '<参数'> '<字符串>'*
```bash
%s                              # 指定输出字符串
%d %i                           # 指定输出十进制整数
%o                              # 指定输出八进制 整数
%x %X                           # 指定输出十六进制整数
%f %F                           # 指定输出浮点数
%e %E                           # 以科学计数法输出浮点数
%c                              # 指定输出字符
%%                              # 输出百分号

```

- 转义符参数

*用法: printf '<参数> <字符串>'*
```bash
\"                              # 输出双引号
\\                              # 输出反斜杠
%%                              # 输出百分号
\n                              # 输出换行符
\t                              # 输出水平制表符
\v                              # 输出垂直制表符
\r                              # 输出回车
\f                              # 输出换页
\a                              # 发出警报
\b                              # 输出退格
\e                              # 删除右边一个字符

```

- 颜色参数
*用法: printf '<属性><字符串><重置属性>'*
```bash
# 设置属性
\033[0m                         # 重置文本属性,关闭闪烁和反显
\033[1m                         # 设置粗体
\033[3m                         # 设置斜体
\033[4m                         # 设置下划线
\033[5m                         # 设置闪烁
\033[7m                         # 设置反显

# 设置背景色
\033[40m                        # 黑色
\033[41m                        # 红色
\033[42m                        # 绿色
\033[43m                        # 黄色
\033[44m                        # 蓝色
\033[45m                        # 洋红色
\033[46m                        # 青色
\033[47m                        # 白色

# 设置文本颜色
\033[30m                        # 黑色
\033[31m                        # 红色
\033[32m                        # 绿色
\033[33m                        # 黄色
\033[34m                        # 蓝色
\033[35m                        # 洋红色
\033[36m                        # 青色
\033[37m                        # 白色

```
- 格式化输出

```bash
printf "%-10s %d %-10.2f \n" xiaomin 2 2.55 # 格式化输出 
printf "%-10s"                              # "-"表示左对齐,默认右对齐
printf "%.10s"                              # 超过十个字符的内容不显示
printf "%010s"                              # 字符补位,不足十位以 "0" 补位
printf "%.10s..."                           # 字符截断,超过十个字符的内容输出 "..."
printf "%+10s"                              # "+"输出加号
printf "%0.2f"                              # "m.n"表示浮点数格式位
printf "%10s"                               # "10"表示字符串格式位,表示该字符串需要使用几个位置
printf "%10s \n" "abc" | tr ' ' '-'         # 利用 "tr" 进行替换输出字符,将空格转换为"-"
printf "\033[1mAomesad\033[0m"              # 颜色参数设置,结尾需要重置参数 "\033[0m"
```

# 脚本参数

*向脚本传递参数,与函数传参相同*
```bash
$0                              # 表示当前执行文件名（包含路径）
$1 $2 .. $n                     # 表示第几个位置参数
$#                              # 获取输入参数的个数,常用于循环
$*                              # 以一个单字符串显示所有向脚本传递的参数。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
$$                              # 脚本运行的当前进程ID号
$!                              # 后台运行的最后一个进程的ID号
$@                              # 与$*相同,但是使用时加引号,并在引号中返回每个参数。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
$?                              # 显示最后命令的退出状态。0表示没有错误,其他任何值表明有错误。

./test.sh 1 2 3 ...
```

# if 判断

* 基础用法 1

```bash
if [condition]
then
    command
    command
    ...
    command
else
    command
fi
```

* 基础用法 2

```bash
if [condition]
then
    command
elif condition
then
    command
elif condition
then 
    command
else
    command
fi
```

* 示例 1

```bash
if [[ $a = 1 || $b = 2 ]]       # 双方括号可以使用 || && 和计算符号 <> < > = != + - / * ^
if [ $a -a $b ]                 # 单方括号需要使用 -a -o ! -eq 等
if [ $a ] || [ $b ]             # 使用 || &&
if [ $a ] && [ $b ]             # 使用 &&
if (( "$a" < "$b" ))            # (())内可以使用计算符号<> < > = != + - / * ^
if (( )) || (())
```

* 示例 2

```bash
if test $[] -eq $[]                   # "-eq" 类型判断
then
 echo ""
else
 echo ""
fi

if test $[] = $[]                     # "=" 类型判断
then
 echo ""
else
 echo ""
fi

if test -e ./bash
then
    echo '文件已存在!'
else
    echo '文件不存在!'
fi

if [ test -e ./notFile -o -e ./bash ] # Shell 还提供了与( -a )、或( -o )、非( ! )三个逻辑操作符用于将测试条件连接起来,其优先级为： ! 最高, -a 次之, -o 最低
then
    echo '至少有一个文件存在!'
else
    echo '两个文件都不存在'
fi
```

* 示例 3

```bash
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi

if [ $str1 -eq $str2 ];then if
```

# case 判断

```bash
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1 )  
    echo '你选择了 1'
    ;;
    2 )  
    echo '你选择了 2'
    ;;
    3 )  
    echo '你选择了 3'
    ;;
    4 )  
    echo '你选择了 4'
    ;;
    * )  
    echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

# for 循环

*items 可以是列表,数组,命令结果等*

```bash
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done

for var in item1 item2 ... itemN; do command1; command2… done;
for ((i = 0 ; i < 10 ; i++));do echo "hello!";done
```

# while 循环

- 基础用法 1

```bash
int=1
while(( $int<=5 ))               # condition为True则command执行,直到condition为False时则停止
do
    echo $int
    let "int++"
done
```

* 基础用法 2

```bash
echo '按下 <CTRL-D> 退出'
echo -n '输入你最喜欢的网站名: '
while read FILM
do
    echo "是的！$FILM 是一个好网站"
done
```

- 基础用法3
```bash
int=1
while [ $i -le 100 ]              # condition为True则command执行,直到condition为False时则停止
do
    echo $int
    let "int++"
done

```

* 无限循环

```bash
while :                         # 无限循环
do
    command
done

while true                      # 无限循环
do
    command
done

for (( ; ; ));do                # 无限循环
    command
done
```

- break 用法

```bash
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break                   # 跳出所有循环
        ;;
    esac
done
```

- continue 用法

```bash
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字: "
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的!"
            continue                # 跳出当前循环
            echo "游戏结束"
        ;;
    esac
done
```

# until 循环

```bash
until condition                     # 先执行command,直到condition条件为True时停止
do
    command
done
```

# 函数

* 基础用法 1

```bash
[function] funWithReturn(){             # "[function]"为函数关键字,可不写
    local a="2"                         # 局部变量
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"

    return $(($aNum+$anotherNum))
}

funWithReturn
echo "输入的两个数字之和为 $? !"
```

* 基础用法 2

```bash
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第九个参数为 ${9-_} !"         # ${x-_} 表示传递参数为空
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}

funWithParam 1 2 3 4 5 6 7 8 9 34 73    # 调用函数
```

* Shell 包含

```bash
. filename                              # 全局shell环境执行
source filename
```

# 注释

```bash
:<<EOF
    command
EOF
```

# 环境变量

* 全局变量

```bash
export GLOBAL_VARIABLE="This is a global variable"

/etc/environment
/etc/profile                            # 系统启动或终端启动的环境配置文件
/etc/profile.d/                         # 基于不同shell脚本类型执行不同脚本
/etc/bashrc
```

* 局部变量 1

```bash
vi ~/.bashrc
vi ~/.bash_profile
source .bashrc
```

* 局部变量 2

```bash
env                                      # 查询当前环境变量
set                                      # 查看预定义变量和位置变量
echo $path                               # 查看当前搜索路径
PATH=$path:/home/                        # 修改 path 只对当前bash有效
export variable_name=value               # 局部变量
```

# 运行 shell

- 子 shell 方式运行

```bash
./shell.sh
/bin/sh shell.sh
```

- 全局 shell 方式运行

```bash
source ./shell.sh                        # 在父进程中运行shell脚本 
. ./shell.sh                             # 效果同上
```

---

# Shell 命令篇

# grep

grep 命令用于查找文件里符合条件的字符串或正则表达式。

```bash
grep -l "root" /etc/passwd /etc/shadow              # 查找文件中包含"root"的行
grep -n "root" /etc/passwd                          # 显示行号
grep -v "root" /etc/passwd                          # 反选,不包含"root"的行
grep -r "root" /etc                                 # 在文件夹和子文件夹的文件中递归搜索"root"
grep -i "root" /etc                                 # 忽略大小写
grep -w "root" /etc                                 # 仅匹配单词
grep -x "root" /etc                                 # 仅匹配行
grep -e "root" -e "nobody" /etc                     # 搜索多个关键字
grep -f a.txt /root                                 # 从文件中获取参数搜索
grep -m 2 "root" /etc/passwd                        # 最大寻找数
grep -E "^root" /etc/passwd                         # 使用扩展正则表达式
```

# sed

```bash
a 新增 当前行的下一行
c 取代 替换
d 删除 删除整行
i 插入 当前行的上一行
s 取代 正则表达式
//  匹配
/^/ 行首
/$/ 行尾
-e 多次执行
-i 保存

sed -i s/#PermitRootLogin/PermitRootLogin/ /etc/ssh/sshd_config
sed '1,2a\newline' /etc/passwd                      # 当前行的
sed '/linux/,/Linux/i\newline' /etc/passwd
sed 's/linux/Linux/3g' /etc/passwd                  # g表示多次匹配
sed '/linux/c\newline' /etc/passwd
```

# awk

```bash

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

注: 当重复的行并不相邻时,uniq 命令是不起作用的, 需配合 sort 命令使用
```

# join

*将两个文件中指定列内容相同的行合并起来,输出到标准输出*  
*使用注意:连接的文件内容须按照字母或者数字排序*

```bash
-a FILENUM                                          # 显示所有行,FILENUM为"1"或"2",表示为文件1,文件2
-e <EMPTY>                                          # 如两个文件中找不到指定字段(列)的行,则输出该"<>"字符串替代
-i, --ignore-case                                   # 忽略大小写差异
-j FIELD                                            # equivalent to '-1 FIELD -2 FIELD'
-o 1.1                                              # 按照指定文件的字段(列)显示结果,"1.1"表示第一个文件第一个字段(列)
-t <CHAR>                                           # 使用<CHAR>作为字段(列)的分隔符号
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
sort -k 2 test.txt                                  # 按指定的列进行排序
```

# cut

*用于显示每行从开头算起num1至num2的字符*
```bash
-b, --bytes=LIST                                    # 以字节为单位进行分割(N-,N-M,-M)
-c, --characters=LIST                               # 以字符为单位进行分割  
-d, --delimiter=DELIM                               # 指定字段(列)分隔符,默认为制表符  
-f, --fields=LIST                                   # 显示指定字段(列)的内容 
-n                                                  # 不分割多字节多字符,仅与"-b"使用  
--complement                                        # 除被选择列之外的全部列的内容   
-s, --only-delimited                                # 不打印不含分隔符的行  
--output-delimiter=STRING                           # 指定"STRING"为输出字段(列)分割符     
-z, --zero-terminated                               # 行分隔符为NUL,不是换行符      
--help                                              # 显示帮助信息  
--version                                           # 显示版本信息
```

# tr

*用于字符转换,删除,压缩*  
tr [OPTION] SET1 SET2
```bash
-c, -C, --complement                                # 用SET2替换未包含在SET1中的字符
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
tr 'a-z' 'A-Z'
tr -d '0-9'
tr ' ' '-'
tr '0-9' 'a-j'
tr -s ' '
tr -s '[av\t]'


```

# expr

```bash

```

# spell

```bash

```

# let

```bash

```

# look

```bash

```

# find

```bash

```

# dd

```bash

```

# fdisk

```bash

```

# mkfs 

```bash

```

# sync 

```bash

```

# mount

```bash

```

# du

```bash

```

# df

```bash

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

```bash

```

# kill

```bash

```

# procinfo

```bash

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

# unset

```bash

```

# hwclock

```bash

```

# set

```bash

```

# crontab

```bash

```

# zip

```bash

```

# tar

```bash

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

# pkill

```bash

```