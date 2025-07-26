- [查看内核版本](#查看内核版本)
- [查看系统版本](#查看系统版本)
- [查看系统信息](#查看系统信息)
- [前后台切换](#前后台切换)
- [文件信息](#文件信息)
- [bash](#bash)
- [定时任务](#定时任务)
- [环境变量](#环境变量)
- [linux 快捷键](#linux-快捷键)
- [sshd\_config 参数](#sshd_config-参数)
- [快速删除](#快速删除)
- [文件加密](#文件加密)
- [man 命令释义查询](#man-命令释义查询)
- [history 历史命令记录使用](#history-历史命令记录使用)
- [文件信息](#文件信息-1)
- [登录提示信息](#登录提示信息)
- [PAM](#pam)
- [时间调整](#时间调整)
- [无交互修改密码](#无交互修改密码)
- [错误处理](#错误处理)
- [正则表达式(PCRE 标准)](#正则表达式pcre-标准)
- [正则表达式(POSIX 标准)](#正则表达式posix-标准)
- [添加用户默认设置](#添加用户默认设置)
- [特殊权限](#特殊权限)
- [umask](#umask)
- [ACL 权限](#acl-权限)
- [权限委派](#权限委派)
- [进程管理](#进程管理)
- [调整进程的优先级](#调整进程的优先级)
- [服务管理](#服务管理)
- [systemd管理服务操作](#systemd管理服务操作)
- [systemd管理target](#systemd管理target)
- [网络管理‍](#网络管理)
- [VLAN](#vlan)
- [bond(链路聚合)](#bond链路聚合)
- [ssh](#ssh)

# 查看内核版本

```bash
cat /proc/version
uname -a
```

# 查看系统版本

```bash
lsb_release -a
cat /etc/issue
cat /etc/redhat-release                                 # 仅限于redhat系linux
cat /etc/os-release                                     # 查看系统版本
```

# 查看系统信息

```bash
fdisk -l                                                # 查看所有分区
disk                                                    # 查看磁盘使用情况
df -lh                                                  # 查看分区使用情况
lsblk                                                   # 磁盘使用情况
free -g                                                 # 查看内存使用情况
free -m                                                 # 查询内存使用量
cat /proc/meminfo                                       # 查看内存信息
env                                                     # 查看环境变量
cat /proc/cpuinfo | grep "Model name"                   # 查询CPU型号
cat /proc/couinfo | grep "physical id" | wc -l          # 物理CPU个数
cat /proc/couinfo | grep "processor" | wc -l            # 总的逻辑核心数
cat /proc/couinfo | grep "cpu cores" | uniq             # 单个CPU的核心数
lscpu | grep "Model name"                               # 根据lscpu查询CPU型号
cat /proc/cpuinfo                                       # 查看CPU信息
dmicode                                                 # 查询BIOS硬件信息
cat /proc/loadavg                                       # 查看系统负载磁盘和分区
lspci | grep -i "eth"                                   # 查询网卡信息
lshw -html > hwhtml.hwhtml                              # 查询硬件信息保存为html文件
mount | column -t                                       # 查看挂载的分区状态
uptime                                                  # 查看系统运行时间、用户数、负载
lsmod                                                   # 列出加载的内核模块
dmesg
dmidecode
```

# 前后台切换

```bash
nohup ./nginx &
jobs -l
```

# 文件信息

```bash
root@ubuntu:~# ll -ash
total 48K
4.0K drwx------.  5 root root 4.0K May  6 15:50 ./
4.0K drwxr-xr-x. 19 root root 4.0K Mar 31 05:41 ../
4.0K -rw-------.  1 root root 1.9K May  6 16:03 .bash_history
4.0K -rw-r--r--.  1 root root 3.1K Oct 15  2021 .bashrc
4.0K drwx------.  2 root root 4.0K May  6 14:33 .cache/
4.0K -rw-r--r--.  1 root root  161 Jul  9  2019 .profile
4.0K drwx------.  3 root root 4.0K Mar 31 05:42 snap/
4.0K drwx------.  2 root root 4.0K Mar 31 05:42 .ssh/
4.0K -rwxr-xr-x.  1 root root 1.3K May  6 14:58 tool.sh*
 12K -rw-------.  1 root root  11K May  6 15:50 .viminfo
```

# bash

```bash
cat -- --bash                           # "--"之后的内容视为文件名参数
```

- bash 脚本写法

```bash
#!/bin/bash -                           # 安全性更高
#!/bin/bash
#!/usr/bin/env bash
```

- 用户命令执行权限目录
  `/etc/sudoers`
  `/etc/sudoers.d/`

```bash
root    ALL=(ALL:ALL) ALL
```

# 定时任务

`/etc/crontab`

- 示例

```bash
crontab -l                              # 查看定时任务
crontab -r                              # 删除定时任务
crontab -u root                         # 设置指定用户的定时任务
crontab -e                              # 编写定时任务

echo "*/2 * * * *  /bin/bash /root/1.sh" >> /var/spool/cron/root
```

- 语法
  > 字符含义:  
  > "\*" 代表取值范围内的数字  
  > "/" 代表每多少  
  > "-" 代表从某数字到某数字  
  > "," 代表分开的数字

|  \*  |  \*  |  \*  |  \*  |  \*  | command |
| :--: | :--: | :--: | :--: | :--: | :-----: |
| 0-59 | 0-23 | 1-31 | 1-12 | 0-6  | command |
|  分  |  时  |  日  |  月  | 星期 |  命令   |

# 环境变量

- 登陆脚本 加载顺序自上而下

```bash
/etc/profile
/etc/bashrc
.bashrc
.profile
```

- 退出脚本

```bash
.bash_history
.bash_logout
```

- nologin 登录脚本

```bash
/etc/bashrc
.bashrc
```

# linux 快捷键

```bash
Ctrl + U                      # 删除当前位置之前的内容
Ctrl + K                      # 删除当前位置之后的内容
Ctrl + Y                      # 恢复删除的内容
Ctrl + D                      # 登出
Ctrl + L                      # 清屏
Ctrl + A                      # 移动光标到行首
Ctrl + E                      # 移动光标到行尾
Ctrl + w                      # 删除光标到左侧的单词
```

# sshd_config 参数

`/etc/ssh/sshd_config`

```bash
AllowUsers zhangsan                     # 允许用户ssh
AllowUsers zhangsan@ip                  # 仅允许指定用户和指定IP组合
AllowGroups                             # 允许用户组ssh
DenyUsers                               # 拒绝用户ssh
DenyGroups                              # 拒绝用户组ssh
PasswordAuthentication                  # 密码验证
PubkeyAuthentication                    # 密钥验证
ChallengeResponseAuthentictaion         # 所有验证方式
ChannelTimeout session:*=1m             # ssh登录1分钟无操作关闭ssh终端
```

# 快速删除

```bash
rm -rf `tar ztf asdasdas.tar.gz`          # " ` ` "符号可以执行命令
```

# 文件加密

```bash
vim -x 1.sh                   # "-x"参数为加密，且脚本无法执行
:set key=                     # "set key="在文本内解密

gzexe 1.sh                    # 加密且可执行，"-d"参数可解密

shc -rvg 1.sh                 # 可加密不解密

tar -zcvf - /var/log | openssl des3 -salt -k redhat | dd of=log.tar.gz                    # 利用openssl加密tar文件
dd if=./log.tar.gz | openssl des3 -salt -k redhat | tar -zxvf -                           # 解密tar文件
```

# man 命令释义查询

```bash
/usr/share/man                          # man命令的目录

whereis man                             # 查看该命令存在man的那个章节
whereis -m man                          # 查看该命令man路径
man 8 vgcreate                          # 跳转章节8查看命令vgcreate的信息
```

# history 历史命令记录使用

```bash
history -c                              # 清除历史命令
history -r                              # 从.bash_hiostory写入内存中
history -w                              # 从内存写入.bash_history文件中
history -d 474                          # 删除指定行的历史命令
$HISTCONTROL                            # 控制历史命令记录形式
```

# 文件信息

```bash
stat tool.sh                                                # 查看文件信息

touch -a/-m/-c -t 90060715.20 /etc/passwd                   # 修改文件时间
```

# 登录提示信息

```bash
/etc/issue                                                  # 本地终端登录前显示信息
/etc/issue.net                                              # 所有终端登录之后显示信息
修改/etc/ssh/sshd_config Banner /etc/issue.net              # 需修改该位置内容

/etc/motd                                                   # 登录后显示信息
```

# PAM

`/etc/pam.d/common-password`

```bash
password requisite pam_cracklib.so retry=3 minlen=9 dcredit=-1 ucredit=-1 lcredit=-1 ocredit=-1 difok=3                 # "minlen"口令最小长度，"dcredit"数字，正数不限制，负数为-N个，"ucredit"大写字母，"lcredit"小写字母，"ocredit"特殊字符，"difok"新密码与旧密码至少有几位不同
```

`/etc/pam.d/common-auth`

```bash
auth required pam_tally2.so deny=3 unlock_time=300
auth required pam_tally2.so deny=3 unlock_time=300 even_deny_root root_ublock_time=300                                  # "deny"错误登次数，"unlock_time"锁定时间，"root_unlock_time"root锁定时间
```

# 时间调整

```bash
timedatectl                                                 # 查看时区
timedatectl set-timezone Asia/Shanghai                      # 时区设置
timedatectl set-time "YYYY-MM-DD HH:MM:SS"                  # 设置时间
timedatectl set-local-rtc 1                                 # 同步硬件时钟，RTC时间

tzselect                                                    # 设置时区
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime         # 复制到localtime
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime      # 复制到localtime
date -R                                                     # 查看时间
date -s "YYYY-MM-DD HH:MM:SS"                               # 设置时间
```

# 无交互修改密码

```bash
#!/bin/bash

# 方法一
NEW_ROOT_PASSWD="YUNtian@2024"

echo "root:$NEW_ROOT_PASSWD" | sudo chpasswd

if [ $? -eq 0 ];then
          echo "修改成功！"
else
          echo "修改失败！" >&2
          exit 1
fi

# 方法二
passwd ubuntu <<EOF
ubuntu
ubuntu
EOF
```

# 错误处理

```bash
command || exit 1

# 写法一
command || { echo "command failed"; exit 1; }

# 写法二
if ! command; then echo "command failed"; exit 1; fi

# 写法三
command
if [ "$?" -ne 0 ]; then echo "command failed"; exit 1; fi

```

# 正则表达式(PCRE 标准)

- 普通字符

| 字符              | 描述                                                                                 |
| :---------------- | :----------------------------------------------------------------------------------- |
| [ABC]             | 匹配"[]"中的所有字符                                                                 |
| [^ABC]            | 匹配除了"[]"中的所有字符                                                             |
| [A-Z] [a-z] [0-9] | 表示区间,匹配所有大写字母                                                            |
| .                 | 匹配除换行符(\n \r)之外的任何单个字符, 等同于"[^\n\r]"                               |
| [\s\S]            | 匹配所有. "\s" 匹配所有空白符, "\S" 匹配非空白符, 不包括换行                         |
| \w                | 匹配字母, 数字, 下划线, 等价于"[A-Za-z0-9_]"                                         |
| \W                | 匹配非字母, 数字, 下划线, 等价于"[^A-Za-z0-9_]"                                      |
| \d                | 匹配任意一个数字, 等价于"[0-9]"                                                      |
| \D                | 匹配非任意一个数字, 等价于"[^0-9]"                                                   |
| \num              | 匹配 "num", 用以对获取的匹配引用, 例如"(.)\1", "\1"获取"(.)", 匹配连续两个相同的字符 |
| (x)               | 匹配括号内的内容并获取这匹配                                                         |
| x\|y              | 或, 匹配 "x" 或 "y", "x \| yood"匹配"x"或"yood", "(x \| y)ood"则匹配"xood"或"yood"   |

- 非打印字符

| 字符 | 描述                                      |
| :--- | :---------------------------------------- |
| \n   | 匹配换行符                                |
| \r   | 匹配回车符                                |
| \s   | 匹配任何空白符, 等价于"[\f\n\r\t\v]"      |
| \S   | 匹配任何非空白字符, 等价于"[^\f\n\r\t\v]" |
| \f   | 匹配换页符                                |
| \t   | 匹配制表符                                |
| \v   | 匹配垂直制表符                            |

- 限定符

| 字符  | 描述                                                                                  |
| :---- | :------------------------------------------------------------------------------------ |
| \*    | 匹配前面的子表达式零或无数次, 等价于"{0,}", 贪婪匹配, 后加"?"变成非贪婪匹配或最小匹配 |
| +     | 匹配前面的子表达式一次或多次, 等价于"{1,}", 贪婪匹配, 后加"?"变成非贪婪匹配或最小匹配 |
| ?     | 匹配前面的子表达式零次或一次, 等价于"{0,1}"                                           |
| {n}   | 匹配确定的 n 次                                                                       |
| {n,}  | 匹配至少 n 次                                                                         |
| {n,m} | 匹配最少 n 次, 最多 m 次                                                              |

- 定位符

| 字符 | 描述                                   |
| :--- | :------------------------------------- |
| ^    | 匹配输入字符串开始的位置, 针对全文首部 |
| $    | 匹配输入字符串结尾的位置, 针对全文结尾 |
| \b   | 匹配一个单词的边界, 字与空格间的位置   |
| \B   | 匹配非单词边界                         |

- 修饰符

| 字符 | 描述                                                                               |
| :--- | :--------------------------------------------------------------------------------- |
| i    | 不区分大小写                                                                       |
| g    | 全局匹配, 查找所有的匹配项                                                         |
| m    | 多行匹配, 使边界字符"^"和"$"匹配每一行的开头和结尾, 多行而非整个字符串的开头和结尾 |
| s    | 特殊字符"."包含换行符"\n"                                                          |


- 实例

```bash
# 普通汉字
[\u4E00-\u9FFF]

# 尽可能所有汉字
[\u4E00-\u9FFF\u3400-\u4DBF\uF900-\uFAFF\U00020000-U0002EBEF]
```

# 正则表达式(POSIX 标准)

适用于 Linux 系统命令

- GNU BRE

| 字符      | 含义                         |
| :-------- | :--------------------------- |
| .         | 匹配任意单个字符             |
| \*        | 匹配前面的子表达式任意次     |
| \\+       | 匹配前面的子表达式一次或多次 |
| \\?       | 匹配前面的子表达式零次或一次 |
| \\{a,b\\} | 匹配区间数量个字符           |
| ^         | 匹配字符串开始               |
| $         | 匹配字符串结尾               |
| \\(\\)    | 分组                         |
| \1..\9    | 管道符多选分支结构           |
| []        | 取值范围                     |
| \|        | 多选分支                     |

- GNU ERE

| 字符   | 含义                         |
| :----- | :--------------------------- |
| .      | 匹配任意单个字符             |
| \*     | 匹配前面的子表达式任意次     |
| +      | 匹配前面的子表达式一次或多次 |
| ?      | 匹配前面的子表达式零次或一次 |
| {a,b}  | 匹配区间数量个字符           |
| ^      | 匹配字符串开始               |
| $      | 匹配字符串结尾               |
| ()     | 分组                         |
| \1..\9 | 管道符多选分支结构           |
| []     | 取值范围                     |
| \|     | 多选分支                     |

# 添加用户默认设置

`/etc/login.defs` : 添加用户后的默认执行的文件, 控制添加用户后的行为设置

# 特殊权限

| 权限    | 操作       | 含义                                                                                                                 |
| :------ | ---------- | :------------------------------------------------------------------------------------------------------------------- |
| SET UID | u+s 或 u+S | 应用于可执行的普通文件, 文件所属者执行权限列有 S 或 S, 任何人执行该文件, 都会临时获得文件所属者的权限                |
| SET GID | g+s 或 g+S | 应用于目录文件, 文件所属组执行权限列有 S 或 S, 任何人新建文件的所属组都会是该所属组, 执行时拥有该组的权限             |
| Sticky  | o+t 或 o+T | 针对写权限的目录,其他组执行权限位有 t 或 T, 只有文件所属组或文件所属者和 root 才能删除文件, 该权限限制了目录的写权限 |


> 注意:
> 特殊权限会覆盖相应的执行权限位, 小写为具备执行权限, 大写为不具备执行权限

# umask

*umask 是一个控制文件创建权限的命令, 它会覆盖文件创建时的默认权限设置, 默认root的 umask 为 022, 普通用户的umask 为 002*

默认的目录权限: 777 rxwrxwrxw
默认的文件权限: 666 rw-rw-rw- (普通文件具有执行权限不安全)

实际默认权限: 默认目录权限减去umask权限: (rwxrwxrwx) - (----wx-wx) = (rwxr--r--)(744)

该值可在系统配置, 用户配置文件中永久更改:
`/etc/profile`
`/etc/bashrc`
`/etc/bash.bashrc`
`/etc/skel`

```bash
umask 022
```

# ACL 权限

*ACL 是一个扩展的权限管理工具, 可以为文件或目录添加额外的权限, 可以为用户组添加权限, 可以为用户添加权限, 可以为特殊权限添加权限*

ACL(Access Control List) 是用户或用户组对文件进行访问进行控制的列表

mask权限表示该目录下可创建的最大权限, 目录设置默认ACL权限表示该目录下创建的文件的默认权限, 该目录下的文件权限将继承目录的权限, 受到mask权限影响

```bash
setfacl -m u:user:rwx /path/to/file
setfacl -m g:user:rwx /path/to/file
setfacl -m o:user:rwx /path/to/file
```

```bash
root@ubtest:~# getfacl cpu_usage.log 
# file: cpu_usage.log
# owner: root
# group: root
user::rw-
group::r--
other::r--

```

# 权限委派

让普通用户获得管理员权限的一种方式

文件: `/etc/sudoers`

sudoers 文件允许特定的用户在执行某些命令时能将权限提升到和root一样, 而非本身为root, 使用的是root的权限而非本身的权限 

# 进程管理

进程至少占用资源: CPU和Memery, 看情况占用network和disk

ps是查看当前进程信息
top是查看动态的进程信息
pstree查看进程树信息

作业调度(jobs), 以shell为单位, shell建立作业不会被另一个shell看到, 可看到作业对应的进程
1. 前台进程(front process)
运行在用户可见可操作的进程叫做前台进程,需要运行其他进程需要关闭前台进程或切换到后台进程
2. 后台进程(backgrand process)
后台进程不会影响前台进程运行, 但并不是所有进程都可以作为后台进程运行

jobs只能查看到当前shell运行的后台进程, 不会查看到其他shell运行的后台进程, 管理范围有限.
jobs使用kill管理后台进程

bg %1 后台进程继续运行
fg %1 后台进程切换到前台运行

Ctrl+C 停止该进程
Ctrl+Z 将进程由前台切换到后台, 该进程状态为Stop, 需要继续运行需要使用Kill发送SIGCONT信号使其继续运行

# 调整进程的优先级

1. 启动时调整优先级
nice调整优先级, nice调整范围为(-20 to 20), PR值不能小于0, nice值越大则优先级越低(正数), nice值越小则优先级越高(负数)
低优先级: nice -n 10 pid
高优先级: nice -n -10 pid
1. 调整已运行进程的优先级
renice -n 10 pid

# 服务管理

服务就是进程, 进程不一定是服务
服务是具备一定功能的进程

linux中第一个进程是整个系统的父进程, 负责所有的进程的启动

init是串行启动, 速度慢, 无法做到根据需要启动服务
systemd是并行启动, 速度快, 难点: 服务之间的依赖问题, 解决办法: 使用类似缓冲池的办法, 将程序放到内存中, 当依赖的服务启动后, 再启动该服务, 减少依赖问题

系统启动流程:(补充)

# systemd管理服务操作

systemd管理的单位为unit
device
mount
scope
service
slice
socket
swap
target
timer
path

```bash
root@ubtest:~# systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; disabled; preset: enabled)
     Active: active (running) since Sat 2025-02-15 04:52:37 UTC; 8h ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 2392 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 2394 (sshd)
      Tasks: 1 (limit: 4555)
     Memory: 4.0M (peak: 4.6M)
        CPU: 29ms
     CGroup: /system.slice/ssh.service
             └─2394 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Feb 15 04:52:37 ubtest systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
Feb 15 04:52:37 ubtest sshd[2394]: Server listening on :: port 22.
Feb 15 04:52:37 ubtest systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
Feb 15 04:52:37 ubtest sshd[2395]: Accepted publickey for root from 172.21.100.209 port 53665 ssh2: RSA SHA256:m/PXyWXYbTdL7Bc2um+w>
Feb 15 04:52:37 ubtest sshd[2395]: pam_unix(sshd:session): session opened for user root(uid=0) by root(uid=0)
Feb 15 04:52:45 ubtest sshd[2634]: Accepted publickey for root from 172.21.100.209 port 53672 ssh2: RSA SHA256:m/PXyWXYbTdL7Bc2um+w>
Feb 15 04:52:45 ubtest sshd[2634]: pam_unix(sshd:session): session opened for user root(uid=0) by root(uid=0)
```

`/usr/lib/systemd/system/ssh.service`
```bash
[Unit]
Description=OpenBSD Secure Shell server
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target auditd.service
ConditionPathExists=!/etc/ssh/sshd_not_to_be_run

[Service]
EnvironmentFile=-/etc/default/ssh
ExecStartPre=/usr/sbin/sshd -t
ExecStart=/usr/sbin/sshd -D $SSHD_OPTS
ExecReload=/usr/sbin/sshd -t
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartPreventExitStatus=255
Type=notify
RuntimeDirectory=sshd
RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
Alias=sshd.service
```

# systemd管理target

target是多个服务组成的一类启动目标, 设置target, 下次系统启动时将会启动该target里面的所有服务

multi-user.target: 字符界面的Linux系统
graphical.target: 图形化界面的linux系统

```bash
# 临时切换到字符界面
systemctl isolate multi-user.target
# 临时切换到图形界面
systemctl isolate graphical.target
# 查看当前启动的target
root@ubtest:~/shelltest# systemctl get-default
graphical.target
```

# 网络管理‍

管理工具填写配置交给服务,服务交给内核,内核处理网络映射等

图形化网卡配置工具: nm-connection-editer
命令行配置文件: NetworkManager nmcli nmtui
               Systemd-networkd netplan

底层配置文件工具  net-tools
                ethtool
                ip
网卡受到网络管理工具管理时, ipconfig直接更改网络配置文件不生效,需要网络管理工具停止管理该网卡配置文件,才能生效,且临时生效

网卡受到网络管理工具管理时, ip直接更改网络配置文件会直接加入到网络管理工具的管理的内存配置文件中, 直接生效,但并未加入实际配置文件中, 临时生效

netstat
ss

# VLAN

服务器与交换机之间的链路为trunk
trunk接口接收带VLAN tag的帧, access接口接收不带VLAN tag的帧, 会丢弃, 只认为出口添加vlan tag
修改内核参数进行转发: net.ipv4.for_ward = 1
/etc/sysctl.conf

# bond(链路聚合)

# ssh

- 加密算法

对称加密算法(加解密密钥相同) n(n-1)/2个
DES(数据加密标准): 3DES(168bit长度加密)
AES(高级数据标准): AES256(256bit长度加密)
RC-6: 可变密钥长度(最长2048bit长度加密)
非对称加密算法(加解密密钥不同) 2n个
RSA算法: 密钥长度不定, 问题: 加密大数据时, 公钥速度远慢于私钥加密

- 防篡改

每个数据包增加校验和, 如自己计算的校验和与数据包附带的校验和不一致, 则数据包被篡改, 将数据包丢弃并重传
MD5(120bit长度 哈希值)
SHA-1(160bit长度 哈希值)
SHA-128
SHA-256
SHA-512

- 数据签名
- 数字证书
- 随机数

ssh服务端有多种非对称密钥适配客户端

登陆时查看服务器指纹, 如指纹不一致, 则服务器伪造
```bash
# /etc/ssh/ 目录下存在密钥文件
ls /etc/ssh/

# 通过如下方式 可查看指纹, -E 指定hash方法
ssh-keygen -lf ssh_host_ecdsa_key -E sha256
```

通过指纹验证的服务器会将服务器的公钥保存到 .ssh/known_hosts 文件中, 下次登录时, 会先查看known_hosts 文件(根据IP地址识别), 如果存在该服务器的公钥, 则不会进行指纹验证, 减少网络开销, 减少服务器伪造的概率

`.ssh/known_hosts`内容格式如下
```bash
192.168.1.1 ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC2+QYZyRQYXZ+KZxYJQYXZ+KZxYJQYXZ+KZxYJQYXZ+KZxYJQYXZ+KZxYJQYXZ+KZxYJQYXZ+KZxYJQYXZ+KZxYJ
```

绕过服务器指纹验证, 仍然会下载服务器的公钥
```bash
ssh -o StrictHostKeyChecking=no root@192.168.1.1
```

ssh登录记录的日志文件
```bash
/var/log/secure
```

ssh远程执行命令, 命令跟在末尾
```bash
ssh root@192.168.1.1 "ls -l"
```

scp
```bash
# 远程拷贝到本地
scp -r root@192.168.1.1:/root/shelltest /root/shelltest
# 本地拷贝到远程
scp -r /root/shelltest root@192.168.1.1:/root/shelltest
```


