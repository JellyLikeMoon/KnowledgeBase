- [1. QAQ答疑](#1-qaq答疑)
  - [Nginx如何处理php请求](#nginx如何处理php请求)
  - [为什么访问nginx出现permission denial](#为什么访问nginx出现permission-denial)
  - [什么是PHP-FPM](#什么是php-fpm)
  - [php-fpm配置略解](#php-fpm配置略解)
- [2. 源码编译安装Nginx](#2-源码编译安装nginx)
  - [源码包](#源码包)
  - [依赖包](#依赖包)
  - [编译安装](#编译安装)
  - [配置Nginx服务](#配置nginx服务)
  - [配置环境变量](#配置环境变量)
  - [命令](#命令)
  - [配置文件（支持PHP）](#配置文件支持php)
- [3. apt安装Nginx](#3-apt安装nginx)
  - [安装](#安装)
  - [配置文件（支持PHP）](#配置文件支持php-1)
  - [命令](#命令-1)
- [4. 常用功能](#4-常用功能)
  - [启用https](#启用https)
  - [虚拟主机](#虚拟主机)
  - [负载均衡](#负载均衡)
  - [反向代理](#反向代理)
  - [动静分离](#动静分离)
  - [Web缓存](#web缓存)
  - [访问控制](#访问控制)
- [5. 其他](#5-其他)
  - [普通用户启动Nginx](#普通用户启动nginx)
  - [自签证书](#自签证书)
  - [配置文件](#配置文件)

---

# 1. QAQ答疑

## Nginx如何处理php请求

Nginx本身不能处理PHP，当接收到PHP请求时，把请求发给fastcgi管理进程处理，fastcgi管理进程选择cgi子进程处理结果并返回给nginx，nginx返回给客户端。

## 为什么访问nginx出现permission denial

apt安装php-fpm的缺省user为www-data  
apt默认安装nginx的缺省user为www-data  
编译安装nginx的缺省user为nginx

需要修改nginx配置文件`/etc/ngin/ngin.conf`
```bash
user www-data
```

## 什么是PHP-FPM

PHP-FPM是一个PHP Fastcgi管理器，仅用于php，官方地址：http://php-fpm.org/  
编译安装PHP时，添加-enable-fpm可以直接集成php-fpm，或者安装
```bash
apt install php8.1-fpm php8.1-cli
```

## php-fpm配置略解

apt安装php-fpm配置文件位于`/etc/php/8.1/fpm/pool.d/www.conf`

```bash
listen.owner = www-data
listen.group = www-data
listen = /run/php/php8.1-fpm.sock 		//此为默认配置，采用sock方式连接，适用于单实例，即php+php-fpm同一台服务器
listen = 127.0.0.1:9000 				//此为采用tcp连接，适用于多实例，即php+php-fpm处于不同服务器，速度受限于tcp
```

`cat /etc/shadow`若不存在上述用户和组则创建

```bash
groupadd www-data
useradd -g www-data www-data
```

---

# 2. 源码编译安装Nginx

## 源码包

```bash
wget http://nginx.org/download/nginx-1.23.1.tar.gz
tar -zxvf nginx-1.23.1.tar.gz -C /opt/
mkdir /etc/nginx
mkdir /usr/local/nginx
```

## 依赖包

* Centos方式

```bash
dnf install -y gcc openssl* zlib* pcre* make
```

* Ubuntu方式

```bash
apt install gcc libpcre3 libpcre3-dev zlib1g-dev openssl libssl-dev make libgd-dev libgeoip-dev
```

## 编译安装

```bash
cd /opt/nginx-1.23.1

./configure
--prefix=/usr/local/nginx
--pid-path=/run/nginx.pid
--conf-path=/etc/nginx/nginx.conf
--http-log-path=/var/log/nginx/access.log
--error-log-path=/var/log/nginx/error.log
--lock-path=/var/lock/nginx.lock
--pid-path=/run/nginx.pid<br />--with-compat
--with-debug
--with-pcre-jit
--with-http_addition_module
--with-http_stub_status_module
--with-http_realip_module
--with-http_auth_request_module
--with-http_v2_module
--with-http_slice_module
--with-http_dav_module
--with-threads
--with-http_geoip_module=dynamic
--with-http_sub_module
--with-http_ssl_module
--with-stream=dynamic
--with-stream_ssl_module
--with-http_gunzip_module
--with-http_gzip_static_module
--with-http_image_filter_module=dynamic

make && make install
```

## 配置Nginx服务

`/lib/systemed/system/nginx.service`  
 ~~/usr/lib/systemd/system/nginx.service~~

```bash
[Unit]
Description=Nginx Web Service
Documentation=https://nginx.org/en/docs
After=network.target
[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx  -t -c /etc/nginx/nginx.conf
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx  -s reload
ExecStop=/usr/local/nginx/sbin/nginx  -s stop
PrivateTmp=true
[Install]
WantedBy=multi-user.target

systemctl daemon-reload
```

注意:  
若编译时未指定PIDFile，默认目录`/usr/local/nginx/logs/nginx.pid`  
apt安装nginx PIDFile文件目录位于`/run/nginx.pid`  
apt安装存在`/etc/init.d/nginx`，若使用则需要修改当前值

```bash
PATH=:/usr/local/nginx/sbin/
DAEMON=/usr/local/nginx/sbin/nginx
PID= /etc/nginx/nginx.conf
```

## 配置环境变量

`/etc/profile`

```bash
export PATH=$PATH:/usr/local/nginx/sbin/

source /etc/profile
ln -s /usr/local/nginx/sbin/nginx /usr/local/sbin/
```

## 命令

```bash
./usr/local/nginx/sbin/nginx                     # 启动
./usr/local/nginx/sbin/nginx -s stop             # 停止
./usr/local/nginx/sbin/nginx -s reload           # 重载配置文件
./usr/local/nginx/sbin/nginx -s quit             # 处理完请求再退出
./usr/local/nginx/sbin/nginx  -t                 # 检查配置文件格式
./usr/local/nginx/sbin/nginx  -V                 # 查看编译信息
./usr/local/nginx/sbin/nginx  -v                 # 查看版本信息
./usr/local/nginx/sbin/nginx  -t                 # 检查配置文件语法正确性
./usr/local/nginx/sbin/nginx  -p                 # 指定Nginx运行目录
./usr/local/nginx/sbin/nginx -c nginx.conf       # 指定启动配置文件
```

## 配置文件（支持PHP）

* `/etc/nginx/nginx.conf`

```bash
user  www-data;
worker_processes  auto;
pid /run/nginx.pid;

events {
worker_connections  1024;
}

http {
include       mime.types;
default_type  application/octet-stream;
sendfile        on;
tcp_nopush     on;
keepalive_timeout  65;
gzip  on;

server {
listen       80;
server_name  localhost;

location / {
root   /var/www/html;
index  index.php index.html index.htm;
try_files $uri $uri/ /index.html;
}

error_page   500 502 503 504  /50x.html;
location = /50x.html {
root   /var/www/html;
}

location ~ \.php$ {

<div>
<!-- 
#  此种方式对应php-fpm中listen = 127.0.0.1:9000使用
#  root      /var/www/html;  
#  fastcgi_pass   127.0.0.1:9000;  
#  fastcgi_index  index.php;  
#  fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;  
#  include        fastcgi_params; -->
</div>

<div>
<!-- 
# 此种方式对应php-fpm中listen = /run/php/php8.1-fpm.sock使用
# fastcgi_pass unix:/run/php/php-fpm.sock;  
# include fastcgi-php.conf; -->
</div>

}
}
}
```

* `/etc/nginx/fastcgi-php.conf`

```bash
fastcgi_split_path_info ^(.+?\.php)(/.*)$;
try_files $fastcgi_script_name =404;
set $path_info$fastcgi_path_info;
fastcgi_param PATH_INFO $path_info;
fastcgi_index index.php;
include fastcgi.conf;
```

* `/etc/nginx/fastcgi.conf`

```bash
fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
fastcgi_param  QUERY_STRING       $query_string;
fastcgi_param  REQUEST_METHOD     $request_method;
fastcgi_param  CONTENT_TYPE       $content_type;
fastcgi_param  CONTENT_LENGTH     $content_length;
fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
fastcgi_param  REQUEST_URI        $request_uri;
fastcgi_param  DOCUMENT_URI       $document_uri;
fastcgi_param  DOCUMENT_ROOT      $document_root;
fastcgi_param  SERVER_PROTOCOL    $server_protocol;
fastcgi_param  REQUEST_SCHEME     $scheme;
fastcgi_param  HTTPS              $https if_not_empty;
fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;
fastcgi_param  REMOTE_ADDR        $remote_addr;
fastcgi_param  REMOTE_PORT        $remote_port;
fastcgi_param  SERVER_ADDR        $server_addr;
fastcgi_param  SERVER_PORT        $server_port;
fastcgi_param  SERVER_NAME        $server_name;
fastcgi_param  REDIRECT_STATUS    200;
```

* `/etc/nginx/fastcgi_params`

```bash
fastcgi_param  QUERY_STRING       $query_string;
fastcgi_param  REQUEST_METHOD     $request_method;
fastcgi_param  CONTENT_TYPE       $content_type;
fastcgi_param  CONTENT_LENGTH     $content_length;
fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
fastcgi_param  REQUEST_URI        $request_uri;
fastcgi_param  DOCUMENT_URI       $document_uri;
fastcgi_param  DOCUMENT_ROOT      $document_root;
fastcgi_param  SERVER_PROTOCOL    $server_protocol;
fastcgi_param  REQUEST_SCHEME     $scheme;
fastcgi_param  HTTPS              $https if_not_empty;
fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;
fastcgi_param  REMOTE_ADDR        $remote_addr;
fastcgi_param  REMOTE_PORT        $remote_port;
fastcgi_param  SERVER_ADDR        $server_addr;
fastcgi_param  SERVER_PORT        $server_port;
fastcgi_param  SERVER_NAME        $server_name;
fastcgi_param  REDIRECT_STATUS    200;
```

---

# 3. apt安装Nginx

## 安装

```bash
apt install nginx
```

## 配置文件（支持PHP）

`/etc/nginx/sites-enabled/default`

```bash
location ~ \.php$ {

<div>
<!--
# 此种方式对应php-fpm中listen = 127.0.0.1:9000使用
root      /var/www/html;  
fastcgi_pass   127.0.0.1:9000;  
fastcgi_index  index.php;  
fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;  
include        fastcgi_params; --> 
</div>

<div>
<!--    
# 此种方式对应php-fpm中listen = /run/php/php8.1-fpm.sock使用
fastcgi_pass unix:/run/php/php-fpm.sock;  
include fastcgi-php.conf; -->
</div>

}
```

## 命令

```bash
systemctl start nginx                     启动
systemctl restart nginx                   重启
systemctl stop nginx                      停止
systemctl enable nginx                    开机自启
systemctl disable nginx                   取消开机自启
systemctl reload nginx                    重载配置文件
```

---
# 4. 常用功能

## 启用https

```bash
server{
      listen 443 ssl;
      server_name localhost;
      ssl_certificate .crt;                     # 公钥
      ssl_certificate_key .key;                 # 私钥
      ssl_ciphers HIGH:!aNULL:!MD5
      ssl_protocols TLSv1.2 TLSv1.3;
      ssl_session_cache shared:SSL:10m;         # 设置SSL缓存及大小
      ssl_session_timeout 5m;                   # 设置SSL会话超时时间
      ssl_session_cache shared:SSL:1m;
      ssl_prefer_server_ciphers  on;
      ssl_password_file /home/cert.pass;        # 若证书有密码,该选项提供密码存储文件位置

      location / {
             root html;
             index index.html index.htm;
      }
}

server {                                  # 80跳转443
       listen 80;
       server_name lzxjack.top;
       rewrite ^(.*)$ https://$host$1;
       location /{

       }
}
```

---


## 虚拟主机

`/etc/nginx/sites-available/default`

```bash
http {
       server {
            listen   80;      							#设置监听地址192.168.72.10
            server_name   localhost;                      
            charset   utf-8;
            #access_log#   logs/www.yuji.access.log;    #设置日志名
            location / {
                   root   /var/www/html/yuji;     		#设置80的工作目录#
                   index   index.html   index.php;
            }
            error_page   500 502 503 504 /50x.html;
            location = 50x.html {
                  root   html;
            }
       }
 
        server {
                    listen   8080;                                    #设置监听地址192.168.72.20#
                    server_name   localhost;                      
                    charset   utf-8;
                    #access_log#   logs/www.nan.access.log;           #设置日志名#
                    location / {
                           root   /var/www/html/nan;                  #设置8080的工作目录
                           index   index.html   index.php;
                    }
                    error_page   500 502 503 504 /50x.html;
                    location = 50x.html {
                          root   html;
                    }
               }
}
```

## 负载均衡
`/etc/nginx/nginx.conf`

- 基于IP负载均衡
```bash
upstram ser {
       server 192.168.0.1:443 weight=10;
       server 192.168.0.2:443 weight=20;
}
server {
       listen 80;
       server_name localhost;
       location / {
              proxy_pass http://ser;
       }
}
```

- 基于端口负载均衡
```bash
upstream node {
       server 192.168.0.1:8081 down max_fails=3 fail_timeout=60s max_conns=1000;
       server 192.168.0.1:8082 backup;
       server 192.168.0.1:8083;

}
server {
       listen 80;
       server_name localhost;
       location / {
              proxy_pass http://node;
       }
}
```

- 配置状态
```bash
down                        # 当前server不参与负载均衡
backup                      # 预留的备份服务器，若其他服务器down，则启用
max_fails                   # 允许请求失败次数，若超过此最大限制，则进入'fail_timeout'时间后从虚拟服务池中kill该服务器
fail_timeout                # 经过'max_fails'失败次数后，服务暂停时间
max_conns                   # 限制最大连接数
```

-  调度策略
```bash
轮询                         # 默认不写为轮询
weight                      # 加权轮询，weight越大分配概率越高
ip_hash                     # 根据访问IP的hash结果分配，分配后该IP将访问固定的后端服务器,会话保持
url_hash                    # (第三方提供)根据访问url的hash结果分配
least_conn                  # 根据连接数分配，优先分配当前连接数最少的服务器
fair                        # (第三方提供)根据后端节点响应时间分配请求，优先分配响应时间短的服务器，nginx本身不支持fair，需下载Nginx模块 "upstream_fair"
hash key [consistent]       # 基于任意关键字的hash算法,模块 "upstream_hash","consistent"参数为一致性hash算法
zone name [size]            # 使策略对所有worker进程生效,设置共享内存大小
```

- 示例(基于url的hash调度策略)(第三方模块提供)
consistent 为一致性hash算法
```bash
upstream server_pool {
       hash $request_uri consistent;
       hash_method crc32;
       server 192.168.0.1:443;
       server 192.168.0.2:443;
}
```

- 示例(基于ip的hash调度策略)
```bash
upstream server_pool {
       ip_hash;
       server 192.168.0.1:443;
       server 192.168.0.2:443;
}
```

- 示例(基于连接数的调度策略)
```bash
upstream server_pool {
       least_conn;
       server 192.168.0.1:443;
       server 192.168.0.2:443;
}
```

- 示例(基于随机的连接数的调度策略)
```bash
upstream server_pool {
       random two least_conn;
       server 192.168.0.1:443;
       server 192.168.0.2:443;
}
```

- 策略对所有worker进程生效
```bash
upstream server_pool {
       least_conn;
       zone name 500;
       server 192.168.0.1:443;
       server 192.168.0.2:443;
}
```

## 反向代理

- 反向代理
```bash
server {
       listen 80;
       server_name localhost;
       location / {
              proxy_pass http://192.168.0.1:8080/;
              # fastcgi_pass 转发至fastcgi服务器
              # scgi_pass 转发至scgi server服务器
              # uwsgi_pass 转发至uwsgi服务器
              # memcached_pass 转发至memcached服务器
              # Nginx主机IP+端口
              proxy_set_header Host $http_host;
              # Nginx主机IP
              # proxy_set_header Host $host;
              # 客户端真实IP
              proxy_set_header X-Real_IP $remote_addr;
              proxy_set_header X-Forwarded-Proto $scheme;
              proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       }
}
```

- 重写

return rewrite重写属于重定向,会重新发起请求,需要明确的IP
```bash
http{
       rewrite_log on;
       rewrite ^/profile/upload http://182.168.0.1:8080$request_uri;
       server {
              listen 80;
              server_name localhost;
              location / {
                     # "permanent" 永久跳转
                     rewrite ^/old(.*)$ /new$1 permanent;
                     # "break" 若当前规则不匹配,停止处理后续rewrite规则,执行当前 "location {}" 内的指令
                     rewrite ^/api/v1(.*)$ /api/v2/$1 break;
                     # "last" 若当前规则不匹配,停止处理后续rewrite规则,使用重写后的路径,重新搜索location及其指令
                     rewrite ^(/download/.*)/media/(\w+)\.?.*$ $1/mp3/$2.mp3 last;
                     proxy_pass http://backed;
              }
       }
}

```

```bash
server {
       listen 80;
       server_name localhost;

       location / {
              return 301 https://192.168.8.1:8080;
              # return 301 "ok!";
              # return 301;
              # return https://localhost:8080;
              return 301 $scheme://www.new.com/$request_uri;
              proxy_pass http://backed;
       }
}
```

- 缓存
```bash
server {
       listen 80;
       server_name localhost;
       location / {
              proxy_cache my_cache;
              proxy_cache_key "$scheme$request_method$host$request_uri";
              proxy_cache_vaild 200 302 1h;
              proxy_pass http://backed;
       }
}
```

- 检查健康状态
```bash
server {
       listen 80;
       server_name localhost;
       location / {
              proxy_cache my_cache;
              proxy_cache_key "$scheme$request_method$host$request_uri";
              proxy_cache_vaild 200 302 1h;
              proxy_pass http://backed;

              health_check uri=/health-check;
       }
}
```

- https反向代理
```bash
server {
       listen 443;
       server_name localhost;
       ssl_certificate ./x.crt;
       ssl_certificate_key ./x.key;

       location / {
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection "upgrade";
              proxy_set_header Host $host;
              proxy_set_header X-Real-IP $remote_addr;
              proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
              proxy_set_header X-Forwarded-Proto https;
              proxy_pass http://backed;
       }
}
```

- TCP代理
```bash
stream{
       upstream backup {
              server localhost:3306;

              # 空闲连接数量8
              keepalive 8;
              # 连接空闲时间超过此值则关闭连接
              keepalive_timeout 120s;
       }
       server{
              listen 3306;
              # proxy_pass backup;
              proxy_pass localhost:3306;
       }
}
```


## 动静分离

- 请求分离
```bash
sever {
       location / {
              proxy_pass http://127.0.0.1:8080;
       }
       
       location ~ \.(css|js|png|jpg|gif|ico) {
              root /home/www/static;
       }

       location ^~ /frons/ {
              root /home/www/static;
       }

       location = /html/ie.html {
              root /home/www/static;
       }
}
```

- localtion修饰符
```bash
=             # 精确匹配,匹配优先级最高
^~            # 前缀匹配普通字符匹配,使用前缀匹配,匹配成功则不再匹配其他location,匹配级别次高
~             # 正则匹配,区分大小写
~*            # 正则匹配,不区分大小写
```

## Web缓存

buffer指的是存储在内存中的数据  
cache指的是存储在磁盘中的数据

- buffer
```bash
server{
      listen 80;
      server_name locahost;
      
      location / {
             # 设置缓冲区数量及大小
             proxy_buffers 16 4k;
             # 设置后端响应的缓冲区大小
             proxy_buffer_size 4k;
             # 设置临时文件最大值
             proxy_max_temp_file_size 100m;
             # 一次写入临时文件大小
             proxy_temp_file_write_size 10m;
             proxy_pass http://location:8080;
      }
}
```

- cache
```bash
http{
       # 定义缓存路径
       proxy_cache_path /data/nginx/cache keys_zone=mycahe:10m;
       proxy_cache_key "$host$request_uri$cookie_user";
       # 设置请求访问次数10则进入缓存;
       proxy_cache_min_uses 10;
       server{
              location / {
                     # 使用缓存区域
                     proxy_cache mycache;
                     # 设置缓存时间,响应为200 302的请求缓存10分钟
                     proxy_cache_valid 200 302 10m;
                     proxy_cache_valid 404       1m;
                     proxy_cache_valid any       10m;

                     proxy_pass http://location:8080;
              }
       }
}
```

## 访问控制

- 基于IP的访问控制
```bash
server {
       listen 80;
       server_name localhost;
       location / {
              root html;
              index index.html;

              deny 192.168.0.1;
              deny 192.168.1.1/24;
              allow all;
       }
}
```

- 基于Basic Auth认证

```bash
server {
       listen 80;
       server_name localhost;
       location / {
              root html;
              index index.html;
              satisfy any;                       #all满足IP条件和http验证,any二者任选其一即可

              auth_basic "auth access test!";
              auth_basic_user_file /usr/auth_conf;
       }
       location /public/ {
              auth_basic off;
       }
}
```

创建口令文件
```bash
apt install httpd-tools
htpasswd -cm /etc/nginx/auth_conf user1
htpasswd -m /etc/nginx/auth_conf user2
```

---

# 5. 其他

## 普通用户启动Nginx

```bash
setcap 'cap_net_bind_service=+eip' /usr/local/nginx/sbin/nginx
setcap -r nginx                                                # 清除附加权限

chown -R www-data:www-data /usr/local/nginx/sbin/nginx
chown -R www-data:www-data /var/log/nginx
chmod u+s /usr/local/nginx/sbin/nginx
```

`/etc/nginx/ngin.conf`
```bash
user www-data;
```

## 自签证书

- RSA密钥生成

openssl默认密钥格式为PKCS#1
```bash
# 生成RSA私钥(无密码)
openssl genrsa -des3 -out rsa_private.key 2048
# 生成RSA公钥(无密码)
openssl rsa -in rsa_private.key -pubout -out rsa_public.key


# 生成RSA私钥(有密码)
openssl genrsa -aes256 -passout pass:password -out rsa_aes_private.key 2048                       # "-passout"设置私钥密码
# 生成RSA公钥(有密码)
openssl rsa -in rsa_aes_private.key -passin pass:password -pubout -out rsa_public.key             # "-passin"读取私钥密码


# 无加密RSA私钥转加密RSA私钥
openssl rsa -in rsa_private.key -aes256 -passout pass:password -out rsa_aes_private.key
```

- 自签名证书

自签名证书,也称为根证书,可用于签发其他证书,根证书是信任链的顶端,可安装到根证书存储中,使得系统和应用程序信任由他签发的证书
```bash
# 同时生成私钥和证书
openssl req -newkey rsa:4096 -nodes -keyout ca_rsa_private.key -x509 -days 365 -out ca_rsa_cert.crt -subj "/C=CN/ST=GD/L=SZ/O=vihoo/OU=dev/CN=vivo.com/emailAddress=yy@vivo.com"
# 使用已有RSA私钥生成自签证书
openssl genrsa -des3 -passout pass:password -out ca_rsa_private.key 2048
openssl req -new -x509 -days 365 -key ca_rsa_private.key -out ca_rsa_cert.crt
```

- 生成CSR签名请求及CA签名

```bash
# 生成RSA私钥
openssl genrsa -aes256 -passout pass:password -out server.key 2048
# 去除key(应用程序使用时无需密码)
openssl rsa -in server.key -out server.key 
# 生成CSR证书(手动输入信息)
openssl req -new -key server.key -out server.csr
# 生成CSR证书(命令行一键输入信息)
openssl req -new -key server.key -passin pass:password -out server.csr -subj "/C=CN/ST=GD/L=SZ/O=vihoo/OU=dev/CN=vivo.com/emailAddress=yy@vivo.com"

# 通过CA证书及CA私钥对CSR请求签发证书进行签发,生成经由CA签名的CRT证书
# "-CA ca_rsa_cert.crt -CAkey ca_rsa_private.key -passin pass:password -CAcreateserial" 替换为 "-signkey"则为自签名CRT证书
openssl x509 -req -days 3650 -in server.csr -CA ca_rsa_cert.crt -CAkey ca_rsa_private.key -passin pass:password -CAcreateserial -out server.crt

# 查看证书内容
openssl x509 -in server.crt -text -noout

# 证书格式转换
openssl x509 -in cert.cer -inform DER -outform PEM -out cert.pem
```

## 配置文件

- 配置文件结构

**全局块：**  
配置影响Nginx全局的指令，如运行Nginx服务器的用户（组）、工作进程数、错误日志的位置等。  
示例指令：`user nobody;`、`worker_processes 1;`、`error_log logs/error.log;`  
**events块：**  
设定Nginx的工作模式及连接上限，如每个worker进程的最大连接数、是否开启对多worker process下的网络连接进行序列化等。  
示例指令：`worker_connections 1024;`、`multi_accept on; `   
**http块：**  
包含HTTP相关的指令，如文件引入、MIME-Type定义、日志自定义、连接超时时间等。  
可以嵌套多个server块，用于配置不同的虚拟主机。  
示例指令：`include mime.types;`、`sendfile on;`   
**server块：**  
代表一个虚拟主机，可以配置多个，用于支持多个域名或IP地址。  
包含server全局块和多个location块。  
示例指令：`listen 80;`、`server_name localhost;`  
**location块：**  
配置URL路径的指令，可以定义URL路径的匹配规则和处理方式，如反向代理、重定向等。  
示例指令：`location / { root html; index index.html index.htm; }`

- 配置示例

`/etc/nginx/nginx.conf`  
`/etc/nginx/sites-avalable/`
```bash
user www-data;                                          # Nginx启动用户
worker_processes auto;                                  # 指定Nginx启动的工作进程数
events{                                                 # 告诉Nginx如何处理链接
       worker_connections 1024;                         # 指定每个工作进程可处理的最大连接数
}                                                       

gzip on;                                                # 启用压缩响应
gzip_types text/plain application/xml;                  # 压缩其他MIME类型响应
gzip_min_length 1000;                                   # 超过1000字节的文件将压缩

http{
       include /etc/nginx/mime.types                    # 引入文件

       upstream back-servers {                          # "upstream"表示负载均衡,"back-servers"为自定义的访问地址名称
              server localhost:3000 weight=2;           # "server"表示可访问的URL地址,"weight"表示权重,数值越大被访问的概率越大
              server localhost:3001 weight=6;
       }

       server{                                          # 服务器
              listen 80;                                # 监听端口
              server_name localhost;                    # 服务器名称
              root /var/www/html;                       # 网站根目录
              index index.html;                         # 指定默认页面
              error_page 404 /404.html;                 # 重写错误页面

              location /app {                           # 匹配网页位置，根据不同的路径执行不同的操作，"/app" 表示访问文件 " /app/ "表示访问的URL;匹配路径中包含 " app " 字符的URL和文件路径
                     sendfile on;                                            # 直接将文件复制到另一个文件,不经过缓冲区
                     tcp_nopush on;                                          # 与sendfile一起使用
                     try_files $uri /images/default.html = 404;              # 检查文件或目录是否存在,若不存在则重定向到指定位置
                     error_page 404 /404.html;          #指定错误显示页面

              }

              location ~ /app/index[1-2].html {         # " ~ " 表示启用正则表达式; " = " 表示精准匹配
                     root /var/www/html;
              }

              rewrite /temp /app/index.html;            # 重写路径,访问/temp会转到/app/index.html

              location / {
                     try_files $uri $uri/ =404;
              }

              location /next1 {
                     proxy_pass http://localhost:3000;  # "proxy_pass" 表示反向代理
              }

              location /next2 {
                     proxy_pass http://localhost:3001;
              }

              
       }
}
```

- 配置示例
```bash
events{}                                                # 告诉nginx如何处理链接

http{
       include /etc/nginx/mime.types;                   # 引入文件
       autoindex off;
       server_tokens off;
       keepalive_timeout 60;
       send_timeout 600;

       upstream localhost {                             # "upstream"表示负载均衡,"localhost"为自定义的访问地址名称
              server localhost:81 weight=1;             # "weight"表示权重,数值越大被访问的概率越大
              server localhost:82 weight=3;
       }

       server{                                          # 服务器
              listen 81;                                # 监听端口
              # listen *:8081;
              # listen 127.0.0.1:8081
              server_name localhost;                    # 服务器名称
              root /var/www/html/site1;                 # 网站根目录
              index index.html;                         # 指定默认页面

              location / {
                        try_files $uri $uri/ =404;
              }
       }

        server{
                listen 82;
                server_name localhost1;
                root /var/www/html/site2;
                index index.html;

                location / {
                        try_files $uri $uri/ =404;
                }
        }

        server{
                listen 80;
                server_name mylocalhost;

                location / {
                        proxy_pass http://localhost;
                }
        }
}
```

