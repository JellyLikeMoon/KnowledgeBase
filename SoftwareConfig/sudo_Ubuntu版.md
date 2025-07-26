# sudo

- 参数

```bash
sudo -l                                 # 查看当前用户可使用命令
sudo -u username id                     # 查看当前用户ID和GROUP
usermod -a -G sudo username             # 将用户加入sudo组
sudo -u username /bin/bash              # 以当前用户身份执行, 默认为root
sudo -k                                 # 结束密码有效期, 下次执行sudo需要时输入密码
sudo -s                                 # 指定执行shell
```

- 配置文件

`/etc/sudoers`

- 配置文件语法

```bash
User_List Host_List=(Runas_List1:Runas_List2) SELinux_Spec Tag_Spec Cmnd_List

#User_List(必填项): 指的是该规则是针对哪些用户的
#Host_List(必填项): 指的是该规则针对来自哪些主机的用户
#Runas_List1(可选项): 表示可以用sudo -u来切换的用户
#Runas_List2(可选项): 表示可以用sudo -g来切换的用户组
#SELinux_Spec(可选项): 表示SELinux相关的选项, 可选值为ROLE=role或TYPE=type
#Tag_Spec(可选项): 用于控制后面Cmnd_List的一些选项, 可选值有下面这些
'NOPASSWD:' | 'PASSWD:' | 'NOEXEC:' | 'EXEC:' | 'SETENV:' | 'NOSETENV:' | 'LOG_INPUT:' | 'NOLOG_INPUT:' | 'LOG_OUTPUT:' | 'NOLOG_OUTPUT:'|'MAIL:'|'NOMAIL:'|
# 如果Runas_Alias和Runas_Alias都没填的话, 默认是以root用户执行

# 通配符
# 通配符只可以用在主机名、文件路径、命令行的参数列表中。下面是可用的通配符：
# *：匹配任意数量的字符
# ?：匹配一个任意字符
# [...]：匹配在范围内的一个字符
# [!...]：匹配不在范围内的一个字符
# \x：用于转义特殊字符
# 在使用通配符时有以下的注意点：
# 1.使用[:alpha:]等通配符时，要转义冒号':'，如：[:alpha:]
# 2.当通配符用于文件路径时，不能跨'/'匹配，如：/usr/bin/*能匹配/usr/bin/who但不能匹配/usr/bin/X11/xterm
# 3.如果指令的参数列表是""时，匹配不包含任何参数的指令。
# 4.ALL这个关键字表示匹配所有情况。
```

- 配置文件示例

```bash
root ALL=(ALL:ALL) ALL                                                              # 网络中的主机=(用户:组) 命令路径
username ALL=(ALL) PASSWD:/bin/kill NOPASSWD:/bin/useradd,!/usr/bin/passwd root     # 命令前加！为不允许执行
username localhost=(root) PASSWD:/bin/kill NOPASSWD:/bin/useradd                    # 仅本机无密码执行useradd, 有密码执行kill

%usergroup ALL=(ALL:ALL) ALL                                                        # 为这一组用户赋予权限, 也可用系统自带的组

Host_Alias localhost = 127.0.0.1,localhost                                          # 将多个主机合成一组
User_Alias usergroup = user1,user2                                                  # 将多个用户合成一组
Cmnd_Alias soft = /bin/nice,/bin/kill                                               # 将多个命令合成一组

username ALL = soft                                                                 # 用户username可以使用soft命令组的命令
```

# rbash的使用

> * 无法执行cd命令
> * 无法修改环境变量
> * 无法执行包含/字符的程序
> * 无法执行重定向运算符
> * 无法关闭rbash模式
> * 无法退出rbash模式

```bash
useradd test -s /bin/rbash
mkdir /home/test/bin
ln -s /bin/ls /home/test/bin/ls         # 创建test用户可使用命令的软连接, 仅在bin中有的命令方可执行

chown root /home/test/.bash_profile
chmod 755 /home/test/.bash_profile

vi /home/test/.bash_profile
PATH=$HOME/bin      				    # 设置环境变量, 仅允许bin中的命令
```
