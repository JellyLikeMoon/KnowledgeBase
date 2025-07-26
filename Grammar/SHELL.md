- [变量](#变量)
- [字符串操作](#字符串操作)
- [数组](#数组)
- [测试符](#测试符)
- [运算符](#运算符)
- [printf](#printf)
- [if 判断](#if-判断)
- [case 判断](#case-判断)
- [for 循环](#for-循环)
- [while 循环](#while-循环)
- [until 循环](#until-循环)
- [select 选择菜单](#select-选择菜单)
- [函数](#函数)
- [注释](#注释)
- [错误处理](#错误处理)
- [信号和陷阱](#信号和陷阱)
- [环境变量](#环境变量)
- [运行 shell](#运行-shell)
- [调试选项](#调试选项)

---

# 变量

- 变量赋值

```bash
name='name name'                 # '' 定义变量,完全按照字面意思处理,不解析变量或执行命令
name="name name"                 # "" 定义变量可解析变量,执行命令,"=" 两侧不能有空格,如变量中有空格需要 ""或 '' 包含
name=123
```

- 只读变量

```bash

readonly name                    # 只读变量:一旦写入不能改变值,输出只读变量可以不加$
echo name
```

- 全局变量

```bash
export var

```

- 从键盘读取变量

```bash
read name                        # 读取输入的变量
read -p "hello " name            # 显示提示信息
read -t 5 -p "5s" name           # "-t" 等待时间,等待时间内不输入则退出
read -n 2 name                   # "-n" 限制输入的字符数

```

- 取消变量

```bash
unset name                      # 取消变量
```

- 输出变量

```bash
echo $name
echo ${name}                    # "{}" 变量界定符
```

- 命令替换

```bash
tm=$(data)                      # 命令替换(command substitutions): 将命令data的返回值作为变量tm的值
tm=`data`                       # 旧方法, 不建议使用
```

- 特殊变量

```bash
$0                              # 表示当前执行文件名(包含路径)
$1 $2 .. $n                     # 位置参数, 表示传递给脚本或函数的第几个位置参数
$#                              # 获取位置参数的数量,常用于循环
$*                              # 以一个单字符串显示所有向脚本传递的参数
                                # 如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数.
$@                              # 与$*相同,但是使用时加引号,并在引号中返回每个参数
$$                              # 脚本运行的当前进程ID号
$!                              # 后台运行的最后一个进程的ID号
                                # 如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数.
$?                              # 显示最后命令的退出状态, 0表示没有错误,其他任何值表明有错误.
$-                              # 显示当前shell设置的选项标志, 由"set"命令启用或关闭, 用于控制shell的行为方式
                                # h   记录命令历史(history)
                                # i   表示交互式Shell(interactive)
                                # m   支持作业控制(job control)
                                # B   启用了 brace expansion(如 {1..5} 展开)
                                # H   启用了 ! 风格的命令历史替换(history expansion)
                                # s   不记录命令历史(通常用于 #!/bin/sh -s)
                                # t   在执行下一个命令后退出Shell
                                # u   使用未设置变量时报错(等价于 set -u)
                                # x   执行时打印命令(等价于 set -x, 常用于调试)
                                # v   输入每行命令时打印(verbose)
                                # e   命令出错时立即退出(等价于 set -e)
```


# 字符串操作

- 字符串拼接

```bash
str1="book"
str2="hand"
str3=$str1$str2                 # 字符拼接
str3="${str1} ${str2}"          # 字符拼接
str3="String1""String2"         # 字符拼接
```

- 字符串大小写转换

```bash
echo ${distro^^}                # 小写转大写
echo ${distro^}                 # 仅首字母大写
echo ${distro,,}                # 大写转小写
echo ${distro,}                 # 仅首字母小写
echo ${distro^^[jn]}            # 指定字母大写
expr index "$str" "word"        # 查找特定单词或字母的索引值$str

```

- 读取字符串值

```bash
echo ${var1:-word}              # 如var为空或"unset var"则返回"word",否则返回var的值,不改变var的值
echo ${var2:+word}              # 如var被定义,则返回"word",否则返回空, 不改变var的值
echo ${var1:=word}              # 如var为空或"unset var"则返回"word",否则返回var的值,并修改var的值为"word"
echo ${var1:?word}              # 如var为空或被删除,将"word"传送到标准错误输出,检测变量是否被正常赋值
echo ${!v*}                     # 通过前缀为 "V" 的变量,并列出满足条件的变量
```

- 字符串长度

```bash
echo ${#name}                   # 变量的字符长度
```

- 模式替换

```bash
echo ${distro/free/more}        # 将第一个匹配的free替换为more
echo ${distro//free/more}       # 将所有匹配的free替换为more
echo ${distro/#free/more}       # 匹配以"free"开头的字符串并将"free"替换为"more"
echo ${distro/%free/more}       # 匹配以"free"结尾的字符串并将"free"替换为"more"
```

- 模式匹配

```bash
url=www.sina.com.cn
echo ${url#*.}                  # 从前往后删,直到第一个 "."(包含) 之前的内容,最短匹配
echo ${url##*.}                 # 从前往后删,直到最后一个 "."(包含)之前的内容,最长匹配
echo ${url%.*}                  # 从后往前,最短匹配,直到第一个 "."(包含)之后的内容
echo ${url%%.*}                 # 从后往前,最长匹配,直到最后一个 "."(包含)之后的内容

```

- 字符串截取

```bash
echo ${distro:0:6}              # 字符串提取,0表示起始位置,6表示从起始位置开始提取6个字符
echo ${distro:2}                # 字符串提取,从索引2位置开始提取余下所有的字符
echo ${distro:(-2)}             # 截取倒数2位字符
echo ${distro:(-2):5}           # 截取倒数2-5位的字符
```

- 字符串分割

```bash
# 使用空格进行分割
str="hello,world"
arr=(${str//,/ })
```


# 数组

- 定义数组

```bash
array_name=(1 2 3)              # 一次性定义数组,使用空格间隔数组元素

array_name[0]=1                 # 单独定义数组,也是数组赋值
array_name[1]=1
array_name[2]=1

```

- 数组方法

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

- 关联数组

```bash
declare -A array_name=(["googole"]="1234" ["googole"]="1234")   # 关联数组,元素之间以 " " 间隔
```

# 测试符

- 条件判断(POXIS 兼容)

"[]"传统test的替代写法
数值比较: -eq -ne -gt -ge -lt -le
字符串比较: -z -n = !=
文件判断: -e -f -d -r -w -x -s -c -b -p -t -z -n
逻辑判断: -a -o !

```bash
if [ "$a" -eq 1 ]; then
  echo "a is 1"
fi

```

test等同于[], 支持使用"[]"的所有功能

```bash
test $[a] -eq $[b]
test $str1==$str2
test -e /bin/sh
```

- 扩展型条件判断(bash 拓展)

支持"[]"的所有功能, 并且支持逻辑判断
逻辑判断: && || !
模式匹配(通配符): == !=
正则表达式: =~

```bash
if [[ "$a" == 1 || "$b" == 2 ]]; then
  echo "a is 1 or b is 2"
fi

if [[ "$str" =~ ^[0-9]+$ ]]; then
  echo "str是纯数字"
fi

```

# 运算符

- 算术运算

"expr" 仅支持整数运算

```bash
expr $a + $b                    # 加法
expr $a - $b                    # 减法
expr $a \* $b                   # 乘法
expr $a / $b                    # 除法
expr $a % $b                    # 取余
```

"$(())" 仅支持整数运算

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

旧方法, 不建议使用, "$[]" 同 "$(())"

```bash
echo $[$n1+$n2]                 # 使用方法同上$(())

```

"bc" 命令支持小数计算

```bash
val=$(echo "scale=2;3.14/2" | bc)  # "bc" 小数运算,"scale=2" 表示精度为2
echo $val
```

"let" 命令支持变量自加或自减

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

- 关系运算

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

- 逻辑运算

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

- 字符运算

```bash
[ -Z $a ]                       # 字符串为0则返回True
[ -n $a ]                       # 字符串不为0则返回False
[ $a ]                          # 字符串不为空则返回True
[ $a == $b ]                    # 相等
[ $a = $b ]                     # 相等
[ $a != $b ]                    # 不相等
```

- 文件测试

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

- 通配符

```bash
?                               # 单个任意字符
*                               # 匹配0到任意个
[]                              # 匹配任意一个
[A-Z]                           # 取范围
[^]                             # 取反
^                               # 行首限定符
$                               # 行尾限定符

```

- 重定向符

设置 `set -o noclobber` , 使用 ">" 重定向到一个存在的文件会失败, 防止意外覆盖

```bash
>                               # 标准输出:"A > B" A 的输出写入到 B中
>|                              # 标准输出:"A >| B" A 的输出写入到 B 中,如果B存在则强制覆盖
>>                              # 标准输出追加:"A >> B" A 的输出追加到 B 中
2>                              # 标准错误输出:"A >> B" A 的错误输出追加到 B 中
2>>                             # 标准错误输出追加:"A >> B" A 的错误输出追加到 B 中
&>>                             # 重定向标准输出和标准错误输出
<                               # 标准输入
[n]<&m                          # 将文件描述符 n 设为文件描述符 m 的一个副本(输入), n 省略默认为 0
[n]>&m                          # 将文件描述符 n 设为文件描述符 m 的一个副本(输出), n 省略默认为 1
[n]<&- / [n]>&-                 # 关闭文件描述符 n
[n]<>file                       # 使文件 file 在文件描述符 n (默认为0) 上为读写而打开, 文件不存在则创建, 需要对统一文件进行就地读写操作好用
<<END                           # 标准输入追加: "END" 为自定义结束符(开始和结束标识符必须一致, 且结束标识符不能带空格和其他字符)
                                # 如果 "END" 不带引号, 内部会进行参数扩展, 命令替换和算术扩展; 如果被引号包裹, 则按照字面处理, 不扩展
<<-END                          # 如果使用 "<<-" , 则 Here Document 的每一行以及结束分隔符行的前导制表符(tabs)都会被去除
<<<word                         # 用法 'command <<<"string content"' , "word" 会进行参数扩展等, 附加换行符传递给命令的标准输入, 类似 "echo 'hello' | command"
2>&1                            # 标准错误输出转到标准输出
|                               # 管道符, 将左边的输出转为右边的输入, 管道右边的命令总是接收输入的命令
                                # "A | B" A 的输出是 B 的输入
xargs                           # 将管道左边的输出作为参数传递给右边的命令
                                # "A \| xargs B" A 的输出作为参数传递给 B
```

- 进制转换

```bash
16#FF                           # 16 进制
0xFF                            # 16 进制
8#777                           # 8 进制
010                             # 8 进制, "0"开头表示八进制
2#123                           # 2 进制
10#123                          # 10 进制
```


# printf

- 格式化参数
  _用法: printf '<参数>' '<字符串>'_

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

_用法: printf '<参数> <字符串>'_

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
  _用法: printf '<属性><字符串><重置属性>'_

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

# if 判断

使用"[]"时, 变量替换后可能发生词语分裂和路径名扩展, 因此变量通常需要双引号包括以避免引发意外行为
使用"[[]]"时, 内部不会进行词语分裂和路径名扩展, 通常无需对变量进行额外引用(除非变量值本身包括特殊模式字符且用于模式匹配), 支持高级字符串比较, 如模式匹配("="或"==")和正则表达式("=~"), 支持逻辑操作符"&&"和"||"直接在内部使用
支持使用"(())"进行算术条件判断

- 基础用法 1

```bash
if [[ condition ]]
then
    command
    command
    ...
    command
else
    command
fi
```

- 基础用法 2

```bash
if [[ condition ]]
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

- 示例 1

```bash
if [[ $a = 1 || $b = 2 ]]       # 双方括号可以使用 || && 和计算符号 <> < > = != + - / * ^
if [ $a -a $b ]                 # 
if [ $a ] || [ $b ]             # 使用 || &&
if [ $a ] && [ $b ]             # 使用 &&
if (( "$a" < "$b" ))            # (())内可以使用计算符号<> < > = != + - / * ^
if (( )) || (())
```

- 示例 2

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

- 示例 3

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
    1|5 )       # 匹配1或5
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
    * )         # 默认情况
    echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

# for 循环

_items 可以是列表,数组,命令结果等_

```bash
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done

for var in item1 item2 ... itemN; do command1; command2… done       # 列表迭代风格
for ((i = 0 ; i < 10 ; i++));do echo "hello!";done                  # C语言风格 
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

- 基础用法 2

```bash
echo '按下 <CTRL-D> 退出'
echo -n '输入你最喜欢的网站名: '
while read FILM
do
    echo "是的！$FILM 是一个好网站"
done
```

- 基础用法 3

```bash
int=1
while [ $i -le 100 ]              # condition为True则command执行,直到condition为False时则停止
do
    echo $int
    let "int++"
done

```

- 无限循环

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

退出最外层的for while until select循环

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

退出当前循环的剩余部分, 继续下次迭代

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

# select 选择菜单

```bash
select var in list;do
    # 根据用户的选择($var和$REPLY)执行命令
    # 通常在此处使用break退出循环
    commands
    break
done
```


# 函数

- 基础用法 1

```bash
function funWithReturn(){               # "[function]"为函数关键字,可不写
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

- 基础用法 2

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

- 基础用法3
```bash
function funWithParam {                  # 如使用"function"定义函数, 可不使用"()"
    commands
}
```

- 函数返回值
    - 退出状态 (Exit Status):
        - Bash/Zsh/POSIX: 函数的退出状态是其内部执行的最后一条命令的退出状态 . 
        - 可以使用 return n 命令显式指定一个0到255之间的整数作为函数的退出状态 . 0 通常表示成功, 非0表示失败. 
        - 调用函数后, 其退出状态存储在特殊变量 $? 中. 
        - 退出状态主要用于指示函数执行的成功或失败, 适合进行简单的状态检查. 

    - 捕获标准输出 (Capturing Output):
        - Bash/Zsh/POSIX: 如果函数需要返回数据(如字符串、数字列表等), 它应该将数据打印到其标准输出(例如使用 echo 或 printf). 调用者可以使用命令替换 output_variable=$(function_name arg1 arg2) 来捕获这些输出 . 
        - 尝试使用 return "some string" 是不正确的, 因为 return 仅接受数字退出状态. 
        - 需要注意的是, 命令替换通常在子 Shell (subshell) 中执行(尽管 Bash/Zsh 可能对内建命令和函数进行优化以避免子 Shell). 在子 Shell 中设置的变量对父 Shell 是不可见的, 且子 Shell 的创建和销毁会带来一定的性能开销, 尤其是在循环中频繁调用时. 


- Shell 包含

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

# 错误处理

```bash
# 写法一
command || { echo "command failed"; exit 1; }

# 写法二
if ! command; then echo "command failed"; exit 1; fi

# 写法三
command
if [ "$?" -ne 0 ]; then echo "command failed"; exit 1; fi

# 使用陷阱方法
trap 'echo "An error occurred on line $LINENO. Offending command: $BASH_COMMAND"' ERR

```

# 信号和陷阱

- 语法

```bash
# 设置陷阱
trap 'conmand' signal

# 清除陷阱, 将指定信号的处理方式重置为shell启动时的默认行为
trap - signal

# 列出陷阱
trap -l
```

- 特殊陷阱

EXIT(或0): 当shell退出时执行trap定义的命令, 无论正常退出, 通过exit命令退出还是因未捕获的信号而终止, 执行清理操作的理想位置

```bash
TMP_FILE=$(mktemp)
trap 'rm -f "$TMP_FILE"; echo "Script finished, cleaned up $TMP_FILE"' EXIT
#... 脚本主体...
echo "Data" > "$TMP_FILE"
```

RETURN: 每次shell函数或通过"."(source)执行的脚本返回后执行命令

```bash
trap 'echo "A function or sourced script just returned. Last status: $?"' RETURN
my_function() { echo "Running my_function"; return 0; }
my_function
```

DEBUG: 每个简单命令执行之前执行命令, 详细跟踪脚本执行流, 检查变量状态非常有用

```bash
trap 'echo "About to execute: $BASH_COMMAND (line $LINENO)"' DEBUG
#... commands...
```

ERR: 当一个简单命令, 复合命令或shell函数因返回非零退出状态而失败时执行命令

```bash
trap 'echo "Error on line $LINENO: command '\''$BASH_COMMAND'\'' failed with status $?" >&2' ERR
#... commands that might fail...
```

# 环境变量

登录 shell 时配置文件加载顺序:
1. /etc/environment
2. /etc/profile
3. ~/.bash_profile
4. ~/.profile

非登录 shell 时配置文件加载顺序:
1. /etc/bashrc
2. ~/.bashrc

- 系统级变量

```bash
export GLOBAL_VARIABLE="This is a global variable"

/etc/environment                        # 系统环境变量, 所有用户和进程均可访问, 格式为 VAR=value, 不使用export
/etc/profile                            # 系统级登录 shell 环境变量, 用户登陆时执行
/etc/profile.d/                         # 系统级登录 shell 环境变量, 基于不同 shell 脚本类型执行不同脚本
/etc/bashrc                             # 系统级交互式非登录 shell 环境变量, 所有交互式非登录 shell 启动时执行
/etc/bash.bashrc                        # 系统级交互式非登录 shell 环境变量, 所有交互式非登录 shell 启动时执行
```

- 用户级变量 1

```bash
vi ~/.bash_profile                      # 用户登录 shell 时执行的环境配置文件
vi ~/.profile                           # 用户登录 shell 时执行的环境配置文件
vi ~/.bashrc                            # 每次启动交互式非登录 shell 时执行的配置文件
source .bashrc                          # 重载环境变量
```

- 变量查询

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

```bash
( )                                      # 当前主shell下创建子shell运行,子shell环境与主shell不互通
{ command;}                              # 同一个shell进程执行一组命令
```

- 全局 shell 方式运行

```bash
source ./shell.sh                        # 在父进程中运行shell脚本
. ./shell.sh                             # 效果同上
```

# 调试选项

```bash
# 执行命令前, 打印该命令及参数
set -x
set -o xtrace

# 详细模式, shell在读取每行输入后, 会将其原样打印出来, 有助于看到shell实际执行的命令
set -v
set -o verbose

# 错误退出, 如果一个管道, 命令, 列表等等以非零状态退出, shell立即退出, 方便快速发现第一个错误
set -e
set -o errexit

# 未设置变量视为错误, 引用的变量未设置(除了特殊参数@和*), 则视为错误, 打印错误并退出
set -u
set -o nounset

# 管道失败, 管道的返回状态将是最后一个以非零状态退出的命令的返回状态, 如果管道中的命令都执行成功则返回零, 默认情况下, 管道的返回状态是最后一个命令的返回状态.
set -o pipefail

# 语法检查, 读取命令但不执行
set -n
set -o errexit

# 调试时设置
set -euxo pipefail
```



