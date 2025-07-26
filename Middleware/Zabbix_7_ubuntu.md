- [Zabbix 官网](#zabbix-官网)
- [Ubuntu24 仓库源(DEB822 格式)更换](#ubuntu24-仓库源deb822-格式更换)
- [NTP 时间服务器(可选)](#ntp-时间服务器可选)
- [中文环境(可选)](#中文环境可选)
- [Zabbix 仓库源](#zabbix-仓库源)
- [PHP8.3](#php83)
- [Nginx](#nginx)
- [Mariadb](#mariadb)
- [Zabbix-server](#zabbix-server)
- [zabbix-proxy](#zabbix-proxy)
- [Zabbix-Agent2](#zabbix-agent2)
- [高可用部署](#高可用部署)
  - [ZabbixHA 高可用](#zabbixha-高可用)
  - [ZabbixKeepActive 高可用](#zabbixkeepactive-高可用)
- [故障处理](#故障处理)
- [常见操作](#常见操作)

# Zabbix 官网

https://www.zabbix.com/download

# Ubuntu24 仓库源(DEB822 格式)更换

编辑`/etc/apt/sources.list.d/ubuntu.sources`

```bash
Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu
Suites: noble noble-updates noble-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

# 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
Types: deb
URIs: http://security.ubuntu.com/ubuntu/
Suites: noble-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

# NTP 时间服务器(可选)

- 修改配置文件

编辑`/etc/systemd/timesyncd.conf`

```bash
[Time]
NTP=ntp.aliyun.com
FallbackNTP=ntp.ubuntu.com
RootDistanceMaxSec=5
PollIntervalMinSec=32
PollIntervalMaxSec=2048
ConnectionRetrySec=30
SaveIntervalSec=60
```

- 设置时区并启动 NTP 服务

```bash
timedatectl set-timezone Asia/Shanghai
timedatectl set-ntp true
timedatectl status
```

# 中文环境(可选)

该语言环境用于 Zabbix 7.0 的中文支持, 如需使用其他语言环境, 也须一并安装

```bash
apt install language-pack-zh-hans
localectl set-locale LANG=zh_CN.utf8
```

# Zabbix 仓库源

```bash
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
apt update
```

# PHP8.3

- ondrej/php 存储库安装

```bash
sudo apt install software-properties-common -y && sudo add-apt-repository ppa:ondrej/php
```

- php8.3 核心及插件安装

```bash
apt install -y php8.3-common \
php8.3-cli \
php8.3-fpm \
php8.3-opcache \
php8.3-readline \
php8.3-mysql \
php8.3-gd \
php8.3-ldap \
php8.3-curl \
php8.3-mbstring \
php8.3-xml \
php8.3-bcmath \
php8.3-fpm \
php8.3-mysql
```

# Nginx

- Nginx 安装

```bash
apt install -y nginx && systemctl enable nginx && systemctl status nginx
```

- 修改配置文件

允许`php-fpm`接管 Nginx 响应的 php 请求;
编辑`/etc/nginx/sites-available/default`

```bash
server{
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                # With php-fpm (or other unix sockets):
                fastcgi_pass unix:/run/php/php8.3-fpm.sock;
                # With php-cgi (or other tcp sockets):
                #fastcgi_pass 127.0.0.1:9000;
        }

}
```

- 访问测试

```bash
echo "<?php phpinfo(); ?>" > /var/www/html/info.php
curl http://127.0.0.1/info.php
```

# Mariadb

- 官方密钥安装

```bash
sudo apt-get install -y apt-transport-https curl
sudo mkdir -p /etc/apt/keyrings
sudo curl -o /etc/apt/keyrings/mariadb-keyring.pgp 'https://mariadb.org/mariadb_release_signing_key.pgp'

```

- 仓库源设置

编辑`/etc/apt/sources.list.d/mariadb.sources`

```bash
# MariaDB 11.4 repository list - created 2025-02-22 11:34 UTC
# https://mariadb.org/download/
X-Repolib-Name: MariaDB
Types: deb
# deb.mariadb.org is a dynamic mirror if your preferred mirror goes offline. See https://mariadb.org/mirrorbits/ for details.
# URIs: https://deb.mariadb.org/11.4/ubuntu
URIs: https://mirrors.tuna.tsinghua.edu.cn/mariadb/repo/11.4/ubuntu
Suites: noble
Components: main main/debug
Signed-By: /etc/apt/keyrings/mariadb-keyring.pgp
```
> 注意: Zabbix7.0 仅支持MariaDB11.4以下版本

- MariaDB 安装

```bash
sudo apt update && sudo apt install mariadb-server mariadb-client -y
```

- 数据库初始化

MariaDB10.4之后的版本使用`mariadb-secure-installation`进行数据库初始化

```bash
root@zabbix:/etc/apt/sources.list.d# mariadb-secure-installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

- 数据库 root 密码设置

```bash
# mysql -uroot -p
password
mysql> USE mysql;
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'zabbix' WITH GRANT OPTION;
mysql> alter user "root"@"localhost" identified by 'zabbix';
mysql> flush privileges;
```

- zabbix 用户设置

```bash
# mysql -uroot -p
password
mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;
mysql> create user zabbix@localhost identified by 'zabbix';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> set global log_bin_trust_function_creators = 1;
mysql> quit;

```

# Zabbix-server

- zabbix 组件安装

```bash
apt install -y zabbix-server-mysql \
zabbix-frontend-php \
zabbix-nginx-conf \
zabbix-sql-scripts \
zabbix-agent2

```

- agent2 插件安装

```bash
apt install -y zabbix-agent2-plugin-mongodb \
zabbix-agent2-plugin-mssql \
zabbix-agent2-plugin-postgresql

```

> Zabbix 数据库支持 mongodb, mssql, postgresql 但需要安装相应插件支持

- 导入初始架构和数据

```bash
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -h172.21.100.52 -uzabbix -p zabbix

```

- 禁用 log_bin_trust_function_creators 选项

```bash
# mysql -uroot -p
password
mysql> set global log_bin_trust_function_creators = 0;
mysql> quit;

```

- 修改 `zabbix-server` 服务的配置文件

编辑`/etc/zabbix/zabbix_server.conf`

```bash
 LogFile=/var/log/zabbix/zabbix_server.log
LogFileSize=0
PidFile=/run/zabbix/zabbix_server.pid
SocketDir=/run/zabbix
DBHost=172.21.100.52
DBName=zabbix
DBUser=zabbix
DBPassword=zabbix
SNMPTrapperFile=/var/log/snmptrap/snmptrap.log
Timeout=4
FpingLocation=/usr/bin/fping
Fping6Location=/usr/bin/fping6
LogSlowQueries=3000
StatsAllowedIP=127.0.0.1
EnableGlobalScripts=0
```
> 注意: 填写数据库, 主机, 数据库名, 账号, 密码等参数;

- 修改 Zabbix 的 Nginx 配置文件

编辑`/etc/zabbix/nginx.conf`

```bash
server {
        listen          8080;
#        server_name     example.com;
        root    /usr/share/zabbix;
        index   index.php;
        location = /favicon.ico {
                log_not_found   off;
        }
        location / {
                try_files       $uri $uri/ =404;
        }
        location /assets {
                access_log      off;
                expires         10d;
        }
        location ~ /\.ht {
                deny            all;
        }
        location ~ /(api\/|conf[^\.]|include|locale) {
                deny            all;
                return          404;
        }
        location /vendor {
                deny            all;
                return          404;
        }
        location ~ [^/]\.php(/|$) {
                fastcgi_pass    unix:/var/run/php/zabbix.sock;
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_index   index.php;

                fastcgi_param   DOCUMENT_ROOT   /usr/share/zabbix;
                fastcgi_param   SCRIPT_FILENAME /usr/share/zabbix$fastcgi_script_name;
                fastcgi_param   PATH_TRANSLATED /usr/share/zabbix$fastcgi_script_name;

                include fastcgi_params;
                fastcgi_param   QUERY_STRING    $query_string;
                fastcgi_param   REQUEST_METHOD  $request_method;
                fastcgi_param   CONTENT_TYPE    $content_type;
                fastcgi_param   CONTENT_LENGTH  $content_length;

                fastcgi_intercept_errors        on;
                fastcgi_ignore_client_abort     off;
                fastcgi_connect_timeout         60;
                fastcgi_send_timeout            180;
                fastcgi_read_timeout            180;
                fastcgi_buffer_size             128k;
                fastcgi_buffers                 4 256k;
                fastcgi_busy_buffers_size       256k;
                fastcgi_temp_file_write_size    256k;
        }
}
```

- Zabbix 服务重启

```bash
systemctl restart zabbix-server zabbix-agent2 nginx php8.3-fpm
systemctl enable zabbix-server zabbix-agent2 nginx php8.3-fpm

```

- Web 安装 Zabbix 界面

http://127.0.0.1:8080

默认账号:Admin;默认密码:zabbix
![](/Middleware/img/zabbix/登录界面.png)

# zabbix-proxy

- zabbix-Proxy 安装

```bash
apt install zabbix-proxy-mysql zabbix-agent2
```

- 为 zabbix-proxy 配置数据库

```bash
# mysql -uroot -p
password
mysql> create database zabbix_proxy character set utf8mb4 collate utf8mb4_bin;
mysql> create user proxy@localhost identified by 'password';
mysql> grant all privileges on zabbix_proxy.* to proxy@localhost;
mysql> set global log_bin_trust_function_creators = 1;
mysql> quit;
```

- 导入初始架构和数据

```bash
cat /usr/share/doc/zabbix-sql-scripts/mysql/proxy.sql | mariadb -uproxy -p zabbix_proxy
```

- 导入架构和数据后禁用`log_bin_trust_function_creators`选项

```bash
# mysql -uroot -p
password
mysql> set global log_bin_trust_function_creators = 0;
mysql> quit;
```

- 修改`zabbix_proxy`服务的配置文件

编辑`/etc/zabbix/zabbix_proxy.conf`

```bash
ProxyMode=0 # 0: 主动代理模式, 1: 被动代理模式
Server=172.21.100.120 # zabbix-server的IP
Hostname=zabbixb # 代理节点的名称,须与前端节点名称一致
ListenPort=10052 # 代理节点的监听端口
LogFile=/var/log/zabbix/zabbix_proxy.log # 代理节点的日志文件
LogFileSize=0 # 代理节点的日志文件大小
PidFile=/run/zabbix/zabbix_proxy.pid # 代理节点的PID文件
SocketDir=/run/zabbix # 代理节点的Socket目录
DBHost=localhost # 代理节点的数据库服务器IP地址
DBName=zabbix_proxy # 代理节点的数据库名称
DBUser=proxy # 代理节点的数据库用户名
DBPassword=password # 代理节点的数据库密码
ProxyBufferMode=hybrid # 代理节点的缓冲模式
ProxyMemoryBufferSize=16M # 代理节点的缓冲区大小
SNMPTrapperFile=/var/log/snmptrap/snmptrap.log # 代理节点的SNMP trap日志文件
Timeout=4 # 代理节点的监控项超时时间
FpingLocation=/usr/bin/fping # 代理节点的fping路径
Fping6Location=/usr/bin/fping6 # 代理节点的fping6路径
LogSlowQueries=3000 # 客户端的查询日志记录时间
StatsAllowedIP=127.0.0.1 # 允许访问的IP地址
```

- zabbix-proxy 重启

```bash
systemctl enable zabbix-proxy-mysql zabbix-agent2
systemctl restart zabbix-proxy-mysql zabbix-agent2
```

- Web 前端操作

Web 前端 -> 管理 -> proxy 组
![](/Middleware/img/zabbix/添加Proxy组.png)

Web 前端 -> 管理 -> proxy
![](/Middleware/img/zabbix/添加Proxy.png)

> proxy 代理地址填写的 IP 地址为仅允许访问本地 proxy 的 proxyIP:
> 无 proxy 组时为本地 proxyIP 地址
> 加入 proxy 组时需要填写同组所有 proxyIP 地址

Web 前端 -> 数据采集 -> 自动发现 -> 创建发现规则
![新建发现规则](/Middleware/img/zabbix/新建自动发现规则.png)

Web 前端 -> 告警 -> 自动注册动作 -> 创建动作
![新建自动注册动作-动作](/Middleware/img/zabbix/新建自动注册动作-动作.png)
![新建自动注册动作-操作](/Middleware/img/zabbix/新建自动注册动作-操作.png)

Web 前端 -> 监测 -> 自动发现
![自动发现状态](/Middleware/img/zabbix/自动发现状态.png)

Web 前端 -> 监测 -> 主机 -> 主机 -> 选择受到 ProxyGroup 监控
![](/Middleware/img/zabbix/主机信息-主机所属proxy.png)

# Zabbix-Agent2

- zabbix仓库源配置

```bash
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
apt update
```

- zabbix-agent2 安装

```bash
apt install -y zabbix-agent2
```

- 修改`zabbix-agent2`服务的配置文件

编辑`/etc/zabbix/zabbix_agent2.conf`

```bash
PidFile=/var/run/zabbix/zabbix_agent2.pid
LogFile=/var/log/zabbix/zabbix_agent2.log
LogFileSize=0
Server=172.21.100.130,172.21.100.120 # 被动模式下的zabbix-server或zabbix-proxy的IP
ListenPort=10050                     # 客户端的监听端口
ServerActive=172.21.100.120          # 主动模式下的zabbix-server或zabbix-proxy的IP
Hostname=zabbixb
Include=/etc/zabbix/zabbix_agent2.d/*.conf
PluginSocket=/run/zabbix/agent.plugin.sock
ControlSocket=/run/zabbix/agent.sock
Include=/etc/zabbix/zabbix_agent2.d/plugins.d/*.conf
```

> 注意:
> 默认 agent 的 Server = 127.0.0.1, 即指向本机, zabbix-server的agent指向本身或其他server(集群), zabbix-proxy的agent可以指向本身或其他server或其他proxy(proxy组); 
> 如该 agent 需要加入某个 proxy 组而非 proxy 时:
> Server: 必须包含 proxy 组中所有的 proxyIP
> ServerActive: 可以仅包含一个 proxyIP, 但最好包含该 proxy 组中所有的 proxyIP

- zabbix-agent2 重启

```bash
systemctl restart zabbix-agent2 && systemctl enable zabbix-agent2
```

# 高可用部署

## ZabbixHA 高可用

- 修改配置文件

编辑集群中每个`zabbix-server`的`/etc/zabbix/zabbix_server.conf`

```bash
LogFile=/var/log/zabbix/zabbix_server.log
LogFileSize=0
PidFile=/run/zabbix/zabbix_server.pid
SocketDir=/run/zabbix
DBHost=172.21.100.52
DBName=zabbix
DBUser=zabbix
DBPassword=zabbix
SNMPTrapperFile=/var/log/snmptrap/snmptrap.log
Timeout=4
FpingLocation=/usr/bin/fping
Fping6Location=/usr/bin/fping6
LogSlowQueries=3000
StatsAllowedIP=127.0.0.1
EnableGlobalScripts=0
HANodeName=zabbix-node1 # 集群节点名称(Web前端配置节点名时也是此名字,此名称在集群中唯一)
NodeAddress=172.21.100.50:10051 # 集群当前节点IP
```

> 配置Zabbix集群时, 仅需修改`zabbix_server.conf`中的HANodeName和NodeAddress参数即可, HANodeName参数须在集群中唯一.
> 该配置文件须在启动Zabbix Web界面前修改.

- zabbix 安装

浏览器地址栏输入 172.21.100.52:8080, 登录 zabbix 安装界面
![](/Middleware/img/zabbix/zabbix安装界面.png)
检查依赖项
![](/Middleware/img/zabbix/zabbix安装-检查依赖条件.png)
配置数据库
![](/Middleware/img/zabbix/zabbix安装-配置数据库连接.png)
配置集群节点名称
![](/Middleware/img/zabbix/zabbix安装-集群节点名称.png)
安装完成
![](/Middleware/img/zabbix/zabbix安装-安装完成.png)
登录查看集群信息, 当前主节点为 172.21.100.51
![](/Middleware/img/zabbix//zabbix安装-集群信息.png)

## ZabbixKeepActive 高可用

- keepavived 安装

```bash
apt install keepalived && systemctl enable keepalived && systemctl start keepalived
```

- keepavived 配置

主节点:

编辑`/etc/keepalived/keepalived.conf`
```bash
vrrp_track_process chk_nginx { # 定义跟踪进程模块
    process nginx              # 定义追踪进程名 
    weight 10                  # 如该进程停止, 该节点优先级减少
}
vrrp_instance zbx1 {     # 定义VRRP实例模块
    state master         # 指定节点初始状态
    interface ens33      # 指定VRRP实例使用的网络接口
    virtual_router_id 51 # 虚拟路由ID, 值必须唯一, 保证多个vrrp在同网络中不冲突
    priority 244         # 数字越大, 优先级越高, 越容易成为主节点
    advert_int 1         # VRRP广播间隔, 单位为秒
    authentication {     # 定义keepalived认证模块
        auth_type PASS   # 配置为简单密码认证 
        auth_pass zabbix # 配置认证密码
    }
    track_process {      # 定义进程跟踪模块
        chk_nginx        # 引用上文定义的跟踪进程模块
    }
    virtual_ipaddress {  # 定义 VRRP 虚拟IP模块
        172.21.100.55/24
    }
}
```

备节点:

编辑`/etc/keepalived/keepalived.conf`
```bash
vrrp_track_process chk_nginx { # 定义跟踪进程模块
    process nginx              # 定义追踪进程名 
    weight 10                  # 如该进程停止, 该节点优先级减少
}
vrrp_instance zbx1 {     # 定义VRRP实例模块
    state backup         # 指定节点初始状态
    interface ens33      # 指定VRRP实例使用的网络接口
    virtual_router_id 51 # 虚拟路由ID, 值必须唯一, 保证多个vrrp在同网络中不冲突
    priority 243         # 数字越大, 优先级越高, 越容易成为主节点
    advert_int 1         # VRRP广播间隔, 单位为秒
    authentication {     # 定义keepalived认证模块
        auth_type PASS   # 配置为简单密码认证 
        auth_pass zabbix # 配置认证密码
    }
    track_process {      # 定义进程跟踪模块
        chk_nginx        # 引用上文定义的跟踪进程模块
    }
    virtual_ipaddress {  # 定义 VRRP 虚拟IP模块
        172.21.100.55/24
    }
}
```

> 注意, 备节点的priority 必须比主节点的优先级小, 且备节点的state为backup
> 后续访问虚拟IP:172.21.100.55即可

- Zabbix MariaDB 高可用


# 故障处理

- 字体乱码

下载黑体字体

```bash
wget https://www.xxshell.com/download/sh/zabbix/ttf/msyh.ttf && cp msyh.ttf /usr/share/zabbix/assets/fonts/msyh.ttf
```

修改配置文件中的字体名称为'msyh'
编辑`/usr/share/zabbix/include/defines.inc.php`

```bash
define('ZBX_GRAPH_FONT_NAME', 'msyh');
define('ZBX_FONT_NAME', 'msyh');
```

# 常见操作

- 添加主机
  ![](/Middleware/img/zabbix//添加主机.png)

- 添加模板
  ![](/Middleware/img/zabbix//新建模板.png)

- 添加监控项
  ![](/Middleware/img/zabbix//新建监控项.png)

- 添加触发器
  ![](/Middleware/img/zabbix//新建触发器.png)

- 添加图形
  ![](/Middleware/img/zabbix//新建图形.png)

- 添加自动发现规则
  ![](/Middleware/img/zabbix//新建模板自动发现规则.png)

- 添加告警动作

- 添加服务 SLA
