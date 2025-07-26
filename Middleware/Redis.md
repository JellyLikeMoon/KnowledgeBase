# 官网

https://redis.io/docs/latest/operate/oss_and_stack/install/install-stack/apt/

# 安装 Redis 8 数据库

```bash
# 安装工具
sudo apt-get install lsb-release curl gpg
# 添加 Redis 官方 GPG 密钥
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
# 修改密钥权限
sudo chmod 644 /usr/share/keyrings/redis-archive-keyring.gpg
# 添加 Redis 官方APT源
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
# 更新软件包
sudo apt-get update
# 安装 Redis
sudo apt-get install redis

```

服务验证

```bash
manager@server:~$ ss -lntp | grep 6379
LISTEN 0      511        127.0.0.1:6379      0.0.0.0:*
LISTEN 0      511            [::1]:6379         [::]:*

root@server:~# redis-cli
127.0.0.1:6379> ping
PONG

```

# 开启认证

1. 编辑配置文件`/etc/redis/redis.conf`
```bash
# 配置登录密码
requirepass 3xha84hasd8x
```

2. 配置生效
```bash
# 重启redis服务
systemctl restart redis.service

# 手动启动方式
redis-server /etc/redis/redis.conf
```

3. 验证配置

```bash
# 未验证密码登录
root@server:~# redis-cli
127.0.0.1:6379> keys *
(error) NOAUTH Authentication required.

# 登陆时验证密码
root@server:~# redis-cli -a yourStrongPassword123!
127.0.0.1:6379> keys *
(empty array)

# 手动验证密码
root@server:~# redis-cli
127.0.0.1:6379> AUTH yourStrongPassword123!
OK
127.0.0.1:6379> keys *
(empty array)
```

# 开启TLS

1. 创建CA密钥和证书
```bash
openssl genrsa -out ca.key 4096
openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 -out ca.crt \
  -subj "/CN=Redis-CA"
```

2. 生成redis服务端证书
```bash
openssl genrsa -out redis.key 4096
openssl req -new -key redis.key -out redis.csr -subj "/CN=redis-server"
openssl x509 -req -in redis.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
  -out redis.crt -days 3650 -sha256
```

3. 启用TLS

编辑配置文件`/etc/redis/redis.conf`
```bash
# 关闭非加密端口
port 0

# 启用 TLS
tls-port 6379
# 证书目录
tls-c redis.crt
# 密钥目录
tls- redis.key
# CA证书目录
tls-ca-c ca.crt
# 开启客户端验证身份
tls-auth-clients yes

```

修改文件权限
```bash
chmod 600 redis.key
chown redis:redis redis.*
```

启动Redis服务
```bash
redis-server /etc/redis/redis.conf
systemctl restart redis
```

登录Redis服务
```bash
root@server:~# redis-cli --tls --cert /etc/redis/redis.crt --key /etc/redis/redis.key --cacert /etc/redis/ca.crt
127.0.0.1:6379> AUTH  923hkas0cuiasehk310sdfhask
OK
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379>

```


