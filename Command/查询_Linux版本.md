## 查看内核版本

```bash
cat /proc/version
uname -a
```

## 查看系统版本

```bash
lsb_release -a
cat /etc/issue
cat /etc/redhat-release 			                    # 仅限于redhat系linux
cat /etc/os-release
```

## 查看系统信息

```bash
fdisk -l                                          # 查看所有分区
disk                                              # 查看磁盘使用情况
df -lh                                            # 查看分区使用情况
lsblk                                             # 磁盘使用情况

free -g                                           # 查看内存使用情况
free -m                                           # 查询内存使用量
cat /proc/meminfo                                 # 查看内存信息

env                                               # 查看环境变量

cat /proc/cpuinfo | grep "Model name"             # 查询CPU型号
cat /proc/couinfo | grep "physical id" | wc -l    # 物理CPU个数
cat /proc/couinfo | grep "processor" | wc -l      # 总的逻辑核心数
cat /proc/couinfo | grep "cpu cores" | uniq       # 单个CPU的核心数
lscpu | grep "Model name"                         # 根据lscpu查询CPU型号
cat /proc/cpuinfo                                 # 查看CPU信息

dmicode                                           # 查询BIOS硬件信息
cat /proc/loadavg                                 # 查看系统负载磁盘和分区

lspci | grep -i "eth"                             # 查询网卡信息
lshw -html > hwhtml.hwhtml                        # 查询硬件信息保存为html文件
mount | column -t                                 # 查看挂载的分区状态
uptime                                            # 查看系统运行时间、用户数、负载
lsmod                                             # 列出加载的内核模块

dmesg
dmidecode
```

## 前后台切换

```bash
nohup ./nginx &
jobs -l
```

## 文件信息

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

## ssh

`/etc/ssh/sshd_config`

```bash
AllowUsers zhangsan                     # 允许用户ssh
AllowGroups                             # 允许用户组ssh
DenyUsers                               # 拒绝用户ssh
DenyGroups                              # 拒绝用户组ssh
```

## bash

```bash
cat -- --bash                           # "--"之后的内容视为文件名参数
```

- bash脚本写法

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

## 定时任务

```bash
crontab -e                              # 编写定时任务

echo "*/2 * * * *  /bin/bash /root/1.sh" >> /var/spool/cron/root
```

## 环境变量

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

- nologin登录脚本

```bash
/etc/bashrc
.bashrc
```

## linux快捷键

```bash
Ctrl + U                      # 删除当前位置之前的内容
Ctrl + K                      # 删除当前位置之后的内容
Ctrl + Y                      # 恢复删除的内容
Ctrl + D                      # 登出
```

## 快速删除

```bash
rm -rf `tar ztf asdasdas.tar.gz`                  # " `` "符号可以执行命令
```

## 文件加密

```bash
vim -x 1.sh                   # "-x"参数为加密，且脚本无法执行
:set key=                     # "set key="在文本内解密

gzexe 1.sh                    # 加密且可执行，"-d"参数可解密

shc -rvg 1.sh                 # 可加密不解密

tar -zcvf - /var/log | openssl des3 -salt -k redhat | dd of=log.tar.gz                    # 利用openssl加密tar文件
dd if=./log.tar.gz | openssl des3 -salt -k redhat | tar -zxvf -                           # 解密tar文件
```

## man

```bash
/usr/share/man                          # man命令的目录

whereis man                             # 查看该命令存在man的那个章节
whereis -m man                          # 查看该命令man路径
man 8 vgcreate                          # 跳转章节8查看命令vgcreate的信息
```

## history

```bash
history -c                              # 清除历史命令
history -r                              # 从.bash_hiostory写入内存中
history -w                              # 从内存写入.bash_history文件中
history -d 474                          # 删除指定行的历史命令
$HISTCONTROL                            # 控制历史命令记录形式
```

## 文件信息

```bash
stat tool.sh                                                # 查看文件信息

touch -a/-m/-c -t 90060715.20 /etc/passwd                   # 修改文件时间
```

## 登录提示信息

```bash
/etc/issue                                                  # 本地终端登录前显示信息
/etc/issue.net                                              # 所有终端登录之后显示信息
修改/etc/ssh/sshd_config Banner /etc/issue.net              # 需修改该位置内容

/etc/motd                                                   # 登录后显示信息
```

## PAM

`/etc/pam.d/common-password`

```bash
password requisite pam_cracklib.so retry=3 minlen=9 dcredit=-1 ucredit=-1 lcredit=-1 ocredit=-1 difok=3         # "minlen"口令最小长度，"dcredit"数字，正数不限制，负数为-N个，"ucredit"大写字母，"lcredit"小写字母，"ocredit"特殊字符，"difok"新密码与旧密码至少有几位不同
```

`/etc/pam.d/common-auth`

```bash
auth required pam_tally2.so deny=3 unlock_time=300
auth required pam_tally2.so deny=3 unlock_time=300 even_deny_root root_ublock_time=300          # "deny"错误登次数，"unlock_time"锁定时间，"root_unlock_time"root锁定时间
```

## Debain

`/etc/ssh/sshd_config`

```bash
ChannelTimeout session:*=1m                             # ssh登录1分钟无操作关闭ssh终端
```

## 时间调整

```bash
timedatectl                                             # 查看时区
timedatectl set-timezone Asia/Shanghai                  # 时区设置
timedatectl set-time "YYYY-MM-DD HH:MM:SS"              # 设置时间
timedatectl set-local-rtc 1                             # 同步硬件时钟，RTC时间

tzselect                                                # 设置时区
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime     # 复制到localtime
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime  # 复制到localtime
date -R                                                 # 查看时间
date -s "YYYY-MM-DD HH:MM:SS"                           # 设置时间
```

## 无交互修改密码

```bash
#!/bin/bash

NEW_ROOT_PASSWD="YUNtian@2024"

echo "root:$NEW_ROOT_PASSWD" | sudo chpasswd

if [ $? -eq 0 ];then
          echo "修改成功！"
else
          echo "修改失败！" >&2
          exit 1
fi

```
