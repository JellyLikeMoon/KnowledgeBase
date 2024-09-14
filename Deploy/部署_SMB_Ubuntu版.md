# 1 端口

```bash
SMB: TCP 139
CIFS: TCP 445
```

配置文件:  
`/etc/samba/smb.conf`

# 2 samba配置文件解析

security=share //设置用户范文samba服务器的验证方式，一共四种验证方式:

1. share：用户访问samba server不需要提供用户名和密码，安全性较低；
2. user：samba server共享目录只能被授权的用户访问，由samba server负责检查账号和密码的正确性，账号与密码要在本samba server中建立；
3. server：依靠其他windows NT/2000或samba server来验证用户的账号和密码，是一种代理验证方式。该安全模式下，系统管理员可以把所有的windows用户和密码集中到一个NT系统上，使用windows NT进行samba认证，远程服务器可以自动认证全部用户和口令，如果认证失败，samba将使用用户级安全模式作为替代的方式；
4. domain：域安全级别，，使用主域控制器（PDC）来完成认证。

`/etc/samba/smbpasswd`:  
默认并不存在，它是SAMBA预设的使用者密码对应表，文件也可自行在smb.conf里面设定文件名及相应位置。不过需要注意该文件的权限(拥有者为root，权限为600);  
`/var/log/samba`:  
samba服务日志文件

# 3 samba配置文件

> security=user //第101行，以user验证方式访问
>
> [sharing] //自定义共享名称
> comment=this is student directory! //共享描述
> path=/opt/students //共享目录路径
> browseable=yes //yes/no，设置共享是否可浏览，如果no则表示隐藏，需要通过“//ip/共享目录"进行访问
> create mask=0644 //创建的文件权限为644
> directory mask=0755 //创建的文件目录为755
> valid users=zhangsan,lisi //设置允许访问共享的用户，zhangsan、lisi
> write list=zhangsan //设置该共享具有写入权限的用户，（可以为用户和用户组 @group）

# 4 权限设置

> passdb backend=tdbsam //定义用户后台类型
> smbpasswd: 使用SMB服务的smbpasswd命令给系统用户设置SMB密码
> tdbsam： 创建数据库文件并使用pdbedit建立SMB独立用户，smbpasswd -a username 建立samba用户并设置密码，不过需要先建立系统用户，也可以使用pdbedit命令来建立samba用户
> pdbedit -a username：新建samba账号
> pdbedit -x username：删除samba账号
> pdbedit -L：列出samba账号列表，读取passdb.tdb数据库文件
> pdbedit -c “[D]” -u username:暂停该samba用户的账号
> pdbedit -c “[]" -u username:恢复该samba用户的账号
> ldapsam：基于LDAP服务进行账户验证
> username map=/etc/samba/smbusers //配合/etc/samba/smbusers文件设置虚拟账号

# 5 创建smb用户

```bash
useradd -s /sbin/nologin username
passwd username
smbpasswd -a username
```

# 6 创建虚拟用户

```bash
vim /etc/samba/smbusers
username = username1 username2 username3

vim /etc/samba/smb.conf
security = user
username map = /etc/samba/smbusers
```

# 7 服务命令

```bash
systemctl enable smbd
systemctl restart smbd
systemctl start smbd
```

# 安装

```bash
apt install -y samba samba-common daemon libtalloc2   # 安装Samba服务
smbpasswd -a root                                     # 为Smaba设置root用户密码
mkdir /share                                          # 创建共享目录
chmod 777 /share                                      # 修改共享目录访问权限
cp /etc/samba/smb.conf /etc/samba/smb.conf.bak        # 备份原始配置文件
vim /etc/samba/smb.conf                               # 修改配置文件
```

# 配置文件

​`/etc/samba/smb.conf`​

```bash
[global]
   workgroup       =   ROOT  
   server string   =   %h server (Samba, Ubuntu)
   unix charset    =   utf8
   dos charset     =   cp36
   log file        =   /var/log/samba/log.%m
   max log size    =   1000
   load printers   =   no
   security        =   user
   passdb backend  =   tdbsam

[share]
   comment         =   Share Folder
   browseable      =   yes
   path            =   /share
   create mask     =   0700
   directory mask  =   0700
   valid users     =   root
   force user      =   root
   force group     =   root
   public          =   yes
   available       =   yes
   writable        =   yes
   guest ok        =   no
```
