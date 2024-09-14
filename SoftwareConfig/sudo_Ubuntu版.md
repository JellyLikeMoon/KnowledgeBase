---
title: 设置_用户权限_Ubuntu版
date: 2024-06-11T22:52:09Z
lastmod: 2024-06-11T22:52:09Z
---

# sudo

```bash
sudo -l                       //查看当前用户可使用命令
sudo -u username id           //查看当前用户ID和GROUP
usermod -a -G sudo username   //将用户加入sudo组
```

# sudoers文件操作

```bash
root ALL=(ALL:ALL) ALL        //网络中的主机/（用户：组）/命令路径

username ALL=(ALL) NOPASSWD:/bin/kill   //无需自身密码执行
username ALL=(ALL) PASSWD:/bin/kill     //需要自身密码执行
username ALL=(ALL) PASSWD:/bin/kill NOPASSWD:/bin/useradd
username ALL=(ALL) PASSWD:/bin/kill NOPASSWD:/bin/useradd,!/usr/bin/passwd root //命令前加！为不允许执行
username localhost=(root) PASSWD:/bin/kill NOPASSWD:/bin/useradd //仅本机无密码执行useradd，有密码执行kill

User_Alias usergroup = user1,user2      //将多个用户合成一组
%usergroup ALL=(ALL:ALL) ALL            //为这一组用户赋予权限，也可用系统自带的组

Cmnd_Alias soft = /bin/nice,/bin/kill   //将多个命令合成一组
username ALL = soft                     //用户username可以使用soft的命令
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
ln -s /bin/ls /home/test/bin/ls         //创建test用户可使用命令的软连接，仅在bin中有的命令方可执行

chown root /home/test/.bash_profile
chmod 755 /home/test/.bash_profile

vi /home/test/.bash_profile
PATH=$HOME/bin      					//设置环境变量，仅允许bin中的命令
```
