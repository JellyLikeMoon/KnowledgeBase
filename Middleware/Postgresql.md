- [官网](#官网)
- [配置存储库](#配置存储库)
- [安装 PostgreSQL 17 数据库](#安装-postgresql-17-数据库)
- [PostgreSQL 管理操作](#postgresql-管理操作)
- [开启 TLS 验证](#开启-tls-验证)
- [远程访问](#远程访问)
- [升级](#升级)

# 官网

https://www.postgresql.org/download/

# 配置存储库

```bash
sudo apt install -y postgresql-common
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
```

# 安装 PostgreSQL 17 数据库

1. 安装

```bash
apt update && apt upgrade -y
sudo apt install -y postgresql postgresql-client
```

2. 检查服务状态

```bash
systemctl status postgresql
systemctl enable postgresql
```

# PostgreSQL 管理操作

1. 登录数据库

```bash
# 超级用户
root@server:~# sudo -i -u postgres
# 方式一 登录postgresql环境
postgres@server:~$ psql
psql (17.5 (Ubuntu 17.5-1.pgdg24.04+1))
Type "help" for help.
postgres=#

# 方式二 登录postgresql环境
root@server:~# sudo -u postgres psql
psql (17.5 (Ubuntu 17.5-1.pgdg24.04+1))
Type "help" for help.
postgres=#

```

2. 创建数据库

```bash
# 创建数据库
postgres=# create database postgreadmin with encoding='UTF8';
CREATE DATABASE

# 创建数据库并指定用户
postgres=# CREATE DATABASE postgreadmin OWNER postgreadmin;

```

3. 授权

```bash
# 创建用户
postgres=# create user postgreadmin with password 'postgreadmin';
CREATE ROLE

# 为用户授权数据库
postgres=# GRANT ALL PRIVILEGES ON DATABASE postgreadmin TO postgreadmin;
GRANT

# 授权登录权限
postgres=# ALTER ROLE username LOGIN;

# 修改用户密码
jumpserver=# alter user jumpserver password 'jumpserver';
ALTER ROLE

```

4. 查看用户

```bash
# 查看用户 方式一
postgres=# \du
                              List of roles
 Role name  |                         Attributes
------------+------------------------------------------------------------
 jumpserver |
 postgres   | Superuser, Create role, Create DB, Replication, Bypass RLS

# 查看用户 方式二
postgres=# SELECT * FROM pg_user;
  usename   | usesysid | usecreatedb | usesuper | userepl | usebypassrls |  passwd  | valuntil | useconfig
------------+----------+-------------+----------+---------+--------------+----------+----------+-----------
 postgres   |       10 | t           | t        | t       | t            | ******** |          |
 jumpserver |    16389 | f           | f        | f       | f            | ******** |          |
(2 rows)

# 查看用户信息
postgres=# SELECT * FROM pg_roles;
           rolname           | rolsuper | rolinherit | rolcreaterole | rolcreatedb | rolcanlogin | rolreplication | rolconnlimit | rolpassword | rolvaliduntil | rolbypassrls | rolconfig |  oid
-----------------------------+----------+------------+---------------+-------------+-------------+----------------+--------------+-------------+---------------+--------------+-----------+-------
 postgres                    | t        | t          | t             | t           | t           | t              |           -1 | ********    |               | t            |           |    10
 pg_database_owner           | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  6171
 pg_read_all_data            | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  6181
 pg_write_all_data           | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  6182
 pg_monitor                  | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  3373
 pg_read_all_settings        | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  3374
 pg_read_all_stats           | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  3375
 pg_stat_scan_tables         | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  3377
 pg_read_server_files        | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  4569
 pg_write_server_files       | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  4570
 pg_execute_server_program   | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  4571
 pg_signal_backend           | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  4200
 pg_checkpoint               | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  4544
 pg_maintain                 | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  6337
 pg_use_reserved_connections | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  4550
 pg_create_subscription      | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           |  6304
 jumpserver                  | f        | t          | f             | f           | t           | f              |           -1 | ********    |               | f            |           | 16389
(17 rows)

```

5. 查看数据库信息

```bash
postgres=# SELECT datname, datdba, encoding, datcollate, datctype FROM pg_database;
  datname   | datdba | encoding | datcollate  |  datctype
------------+--------+----------+-------------+-------------
 postgres   |     10 |        6 | en_US.UTF-8 | en_US.UTF-8
 template1  |     10 |        6 | en_US.UTF-8 | en_US.UTF-8
 template0  |     10 |        6 | en_US.UTF-8 | en_US.UTF-8
 jumpserver |     10 |        6 | en_US.UTF-8 | en_US.UTF-8
(4 rows)

postgres=# \l
                                                       List of databases
    Name    |  Owner   | Encoding | Locale Provider |   Collate   |    Ctype    | Locale | ICU Rules |    Access privileges
------------+----------+----------+-----------------+-------------+-------------+--------+-----------+-------------------------
 jumpserver | postgres | UTF8     | libc            | en_US.UTF-8 | en_US.UTF-8 |        |           | =Tc/postgres           +
            |          |          |                 |             |             |        |           | postgres=CTc/postgres  +
            |          |          |                 |             |             |        |           | jumpserver=CTc/postgres
 postgres   | postgres | UTF8     | libc            | en_US.UTF-8 | en_US.UTF-8 |        |           |
 template0  | postgres | UTF8     | libc            | en_US.UTF-8 | en_US.UTF-8 |        |           | =c/postgres            +
            |          |          |                 |             |             |        |           | postgres=CTc/postgres
 template1  | postgres | UTF8     | libc            | en_US.UTF-8 | en_US.UTF-8 |        |           | =c/postgres            +
            |          |          |                 |             |             |        |           | postgres=CTc/postgres
(4 rows)


```

6. 选择数据库

```bash
# 选择数据库
postgres=# \c jumpserver
You are now connected to database "jumpserver" as user "postgres".

# 登陆时选择数据库
root@server:~# psql -U postgres -d my_database

```

7. 查看表信息

```bash
# 选择数据库
postgres=# \c jumpserver
You are now connected to database "jumpserver" as user "postgres".

# 查看表信息
jumpserver-# \dt
Did not find any relations.
```

# 开启 TLS 验证

1. 创建证书

```bash
# 生成服务器密钥
openssl genpkey -algorithm RSA -out server.key
# 生成服务器证书签名请求(可选)
openssl req -new -key server.key -out server.csr
# 生成自签名服务器证书
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
# 如未执行第二步,则直接生成证书
openssl req -new -x509 -days 365 -key server.key -out server.crt
# 设置文件权限
chmod 0600 server.key
# 移动证书及私钥至postgresql数据目录
mv server.crt /var/lib/postgresql/data/
mv server.key /var/lib/postgresql/data/
chown postgres:postgres /var/lib/postgresql/data/server.crt /var/lib/postgresql/data/server.key
```

2. 修改 postgresql.conf 文件

```bash
# 开启 SSL
ssl = on

# 指定服务器证书文件的路径
ssl_cert_file = 'server.crt'

# 指定服务器私钥文件的路径
ssl_key_file = 'server.key'

# （可选）指定受信任的 CA 证书文件路径，用于客户端证书验证
# ssl_ca_file = 'root.crt'
```

3. 修改 pg_hba.conf 文件
   此配置文件决定连接的类型和认证方法, `hostssl` 只会匹配 ssl 加密连接

```bash
hostssl all all 0.0.0.0/0 md5
```

4. 重启数据库服务

```bash
# 使用 systemd 的系统
sudo systemctl restart postgresql

# 或使用 pg_ctl
pg_ctl restart
```

5. 验证 TLS

```bash
SELECT ssl, version, cipher, bits, client_dn FROM pg_stat_ssl WHERE pid = pg_backend_pid();
```

6. 客户端连接
   SSL 模式: allow perfer require verify-ca verify-full

```bash
psql "host=your_server_ip dbname=your_db user=your_user sslmode=require"

psql "host=your_server_ip dbname=your_db user=your_user sslmode=verify-full sslrootcert=/path/to/server.crt"
```

# 远程访问

修改 postgresql 配置文件`/etc/postgresql/17/main/postgresql.conf`
只允许特定 IP 则修改为 listen_addresses = '192.168.1.1'
允许所有 IP 则修改为 listen_addresses = '\*'

```bash
root@server:~# cat /etc/postgresql/17/main/postgresql.conf | grep -n listen_addresses
60:listen_addresses = '*'               # what IP address(es) to listen on;
```

# 升级

1. 备份文件

```bash
# 示例：使用 pg_dumpall 进行逻辑备份
pg_dumpall -U postgres > /path/to/your/backup/pg16_fulldump.sql
```

2. 安装新版本 postgresql

```bash
sudo apt-get install postgresql-17
```

3. 创建集群(如无集群则创建)

```bash
# 查看集群信息
pg_lsclusters
# 创建集群
sudo pg_createcluster 17 main --start-conf disabled
```

4. 停止数据库

```bash
sudo systemctl stop postgresql
# 或者针对特定版本
sudo systemctl stop postgresql@16-main
sudo systemctl stop postgresql@17-main
```

5. 兼容性检查

```bash
# 切换到 postgres 用户
sudo -i -u postgres

# 运行检查命令 (路径可能因系统而异，请使用 pg_lsclusters 或 find 命令确认)
/usr/lib/postgresql/17/bin/pg_upgrade --check \
           --old-datadir="/var/lib/postgresql/16/main" \
           --new-datadir="/var/lib/postgresql/17/main" \
           --old-bindir="/usr/lib/postgresql/16/bin" \
           --new-bindir="/usr/lib/postgresql/17/bin" \
           --old-options='-c config_file=/etc/postgresql/16/main/postgresql.conf' \
           --new-options='-c config_file=/etc/postgresql/17/main/postgresql.conf'
```

如果输出: `Clusters are compatible` 则表示一切准备就绪

6. 升级

```bash
# 确保仍是 postgres 用户
/usr/lib/postgresql/17/bin/pg_upgrade --link \
           --old-datadir="/var/lib/postgresql/16/main" \
           --new-datadir="/var/lib/postgresql/17/main" \
           --old-bindir="/usr/lib/postgresql/16/bin" \
           --new-bindir="/usr/lib/postgresql/17/bin" \
           --old-options='-c config_file=/etc/postgresql/16/main/postgresql.conf' \
           --new-options='-c config_file=/etc/postgresql/17/main/postgresql.conf'
```

如果显示: `Upgrade Complete` 则表示升级成功

7. 迁移配置

```bash
# 示例：比较并同步配置
# cp /etc/postgresql/16/main/pg_hba.conf /etc/postgresql/17/main/
# cp /etc/postgresql/16/main/postgresql.conf /etc/postgresql/17/main/
```

需要手动更改配置文件的内容

8. 启动新版本

```bash
# 退出 postgres 用户会话
exit
# 启动新服务
sudo systemctl start postgresql@17-main
# 验证状态
sudo systemctl status postgresql@17-main
# 查询数据库版本
psql -V
```

9. 升级后任务

```bash
# 更新集群统计信息
sudo -i -u postgres
./analyze_new_cluster.sh

# 确保仍是 postgres 用户
./delete_old_cluster.sh

# 对于 Ubuntu/Debian
sudo apt-get remove --purge postgresql-16
# 删除残留
rm -r /usr/lib/postgresql/16
```
