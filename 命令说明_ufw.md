### 官网地址

> [https://help.ubuntu.com/community/UFW](https://help.ubuntu.com/community/UFW)

### 安装

```bash
apt install ufw
```

### 命令

```bash
ufw version                   #查看版本
ufw enable                    #启动ufw
ufw disbale                   #禁用ufw
ufw reload                    #重载ufw
ufw reset                     #重置ufw
ufw verbose                   #查看ufw配置
ufw logging on                #开启日志
ufw logging off               #禁用日志
ufw status                    #查看ufw状态
ufw status verbose            #查看ufw配置
ufw status numbered           #查看ufw规则及编号
```

### 配置文件

预设服务配置：/etc/services  
预设规则：/etc/ufw/before.ruls

### 默认策略

```bash
ufw default allow outgoing              #设置默认允许规则，默认允许所有出去
ufw default deny incoming               #设置默认拒绝规则，默认不允许所有进入
```

- 自定义策略

```bash
ufw allow/deny 53/tcp,3389/udp

ufw allow/deny 8080:8090/tcp

ufw allow/deny ssh

ufw allow/deny from 192.168.1.1

ufw allow/deny from 192.168.1.0/24

ufw allow/deny from 192.168.1.1 to any port 80

ufw allow/deny proto tcp from 192.168.1.0/24 to any port 22

ufw allow/deny from 192.168.1.1 to any port 80 proto tcp

ufw delete allow/deny 3389

```

‍
