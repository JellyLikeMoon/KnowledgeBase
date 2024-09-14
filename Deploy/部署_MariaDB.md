## 官网

[https://mariadb.org/download/?t=repo-config](https://mariadb.org/download/?t=repo-config)

## 软件源

```bash
sudo apt-get install apt-transport-https curl
sudo curl -o /etc/apt/trusted.gpg.d/mariadb_release_signing_key.asc 'https://mariadb.org/mariadb_release_signing_key.asc'
sudo sh -c "echo 'deb https://mirrors.aliyun.com/mariadb/repo/10.9/ubuntu jammy main' >>/etc/apt/sources.list"
```

## 配置文件

`/etc/apt/sources.list`
```bash
## MariaDB 10.9 repository list - created 2022-12-18 07:52 UTC
## https://mariadb.org/download/
deb https://mirrors.aliyun.com/mariadb/repo/10.9/ubuntu jammy main
## deb-src https://mirrors.aliyun.com/mariadb/repo/10.9/ubuntu jammy main
deb https://mirrors.aliyun.com/mariadb/repo/10.9/ubuntu jammy main/debug'
```

## 安装

```bash
sudo apt-get update
sudo apt-get install mariadb-server mariadb-client
```


## 主从库

## 语句

```bash
sudo mkdir -p "/opt/only/mysql/conf.d";
sudo mkdir -p "/opt/only/mysql/data";
sudo mkdir -p "/opt/only/mysql/initdb";
sudo mkdir -p "/opt/only/CommunityServer/data";
sudo mkdir -p "/opt/only/CommunityServer/logs";
sudo mkdir -p "/opt/only/CommunityServer/letsencrypt";
sudo mkdir -p "/opt/only/DocumentServer/data";
sudo mkdir -p "/opt/only/DocumentServer/logs";
sudo mkdir -p "/opt/only/MailServer/data/certs";
sudo mkdir -p "/opt/only/MailServer/logs";
sudo mkdir -p "/opt/only/ControlPanel/data";
sudo mkdir -p "/opt/only/ControlPanel/logs";

sudo docker run --net onlyoffice -i -t -d --restart=always --name onlyoffice-mysql-server \
 -v /opt/only/mysql/conf.d:/etc/mysql/conf.d \
 -v /opt/only/mysql/data:/var/lib/mysql \
 -v /opt/only/mysql/initdb:/docker-entrypoint-initdb.d \
 -e MARIADB_ROOT_PASSWORD=YUNtian@2024 \
 -e MARIADB_DATABASE=onlyoffice \
 mariadb

create database onlyoffice_mailserver; 
create user 'onlyoffice_admin'@'%' identified by 'YUNtian@2024';
create user 'mail_admin'@'%' identified by 'YUNtian@2024';
grant all privileges on onlyoffice.* to 'onlyoffice_admin'@'%';
grant all privileges on onlyoffice_mailserver.* to 'mail_admin'@'%';
flush privileges;

sudo docker run --net onlyoffice -i -t -d --restart=always --name onlyoffice-document-server \
 -e JWT_ENABLED=true \
 -e JWT_SECRET=YUNtian@202407 \
 -e JWT_HEADER=AuthorizationJwt \
 -v /opt/only/DocumentServer/logs:/var/log/onlyoffice  \
 -v /opt/only/DocumentServer/data:/var/www/onlyoffice/Data  \
 -v /opt/only/DocumentServer/lib:/var/lib/onlyoffice \
 -v /opt/only/DocumentServer/db:/var/lib/postgresql \
 -v /opt/only/DocumentServer/forgotten:/var/lib/onlyoffice/documentserver/App_Data/cache/files/forgotten \
 onlyoffice/documentserver

sudo docker run --init --net onlyoffice --privileged -i -t -d --restart=always --name onlyoffice-mail-server \
 -e MYSQL_SERVER=172.18.0.2 \
 -e MYSQL_SERVER_PORT=3306 \
 -e MYSQL_ROOT_USER=root \
 -e MYSQL_ROOT_PASSWD=YUNtian@2024 \
 -e MYSQL_SERVER_DB_NAME=onlyoffice_mailserver \
 -v /opt/only/MailServer/data:/var/vmail \
 -v /opt/only/MailServer/data/certs:/etc/pki/tls/mailserver \
 -v /opt/only/MailServer/logs:/var/log \
 -h dyw.com \
 onlyoffice/mailserver

sudo docker run --net onlyoffice -i -t -d --restart=always --name onlyoffice-control-panel \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /opt/only/CommunityServer/data:/app/onlyoffice/CommunityServer/data \
-v /opt/only/ControlPanel/data:/var/www/onlyoffice/Data \
-v /opt/only/ControlPanel/logs:/var/log/onlyoffice onlyoffice/controlpanel

sudo docker run --net onlyoffice -itd --privileged --restart=always --name onlyoffice-community-server -p 81:80 -p 443:443 -p 5222:5222 --cgroupns=host \
 -e MYSQL_SERVER_ROOT_PASSWORD=YUNtian@2024 \
 -e MYSQL_SERVER_DB_NAME=onlyoffice \
 -e MYSQL_SERVER_HOST=172.18.0.2 \
 -e MYSQL_SERVER_USER=onlyoffice_admin \
 -e MYSQL_SERVER_PASS=YUNtian@2024 \
 -e DOCUMENT_SERVER_PORT_80_TCP_ADDR=172.18.0.3 \
 -e DOCUMENT_SERVER_JWT_ENABLED=true \
 -e DOCUMENT_SERVER_JWT_SECRET=YUNtian@202407 \
 -e DOCUMENT_SERVER_JWT_HEADER=AuthorizationJwt \
 -e MAIL_SERVER_API_HOST=172.18.0.4 \
 -e MAIL_SERVER_DB_HOST=172.18.0.2 \
 -e MAIL_SERVER_DB_NAME=onlyoffice_mailserver \
 -e MAIL_SERVER_DB_PORT=3306 \
 -e MAIL_SERVER_DB_USER=mail_admin \
 -e MAIL_SERVER_DB_PASS=YUNtian@2024 \
 -e CONTROL_PANEL_PORT_80_TCP=80 \
 -e CONTROL_PANEL_PORT_80_TCP_ADDR=172.18.0.5 \
 -v /opt/only/CommunityServer/data:/var/www/onlyoffice/Data \
 -v /opt/only/CommunityServer/logs:/var/log/onlyoffice \
 -v /opt/only/CommunityServer/letsencrypt:/etc/letsencrypt \
 -v /sys/fs/cgroup:/sys/fs/cgroup:rw \
 onlyoffice/communityserver
 ```