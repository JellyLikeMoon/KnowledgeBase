- [1.关闭firewalld和SELinux](#1关闭firewalld和selinux)
- [2.zabbix官方源](#2zabbix官方源)
- [3.安装数据库](#3安装数据库)
- [4.创建初始数据库](#4创建初始数据库)
- [5.导入数据库](#5导入数据库)
- [6.修改zabbix server配置文件](#6修改zabbix-server配置文件)
- [7.修改时区](#7修改时区)
- [7.配置PHP在nginx](#7配置php在nginx)
- [8.启动服务](#8启动服务)

---

## 1.关闭firewalld和SELinux

```bash
sed -i 's/SELINUX=enforcing/SELINUX=disabled' /etc/selinux/config 			//关闭SELinux
systemctl disable --now firewalld 											//关闭防火墙
hostnamectl set-hostname zabbix 											//修改主机名

如果开启SELinux，CentOS7
setsebool -P httpd_can_connect_zabbix on
If the database is accessible over network (including 'localhost' in case of PostgreSQL), you need to allow Zabbix frontend to connect to the database too:
setsebool -P httpd_can_network_connect_db on
```

## 2.zabbix官方源

```bash
rpm -Uvh https://repo.zabbix.com/zabbix/6.0/rhel/8/x86_64/zabbix-release-6.0-1.el8.noarch.rpm
dnf clean all
dnf install zabbix-server-mysql zabbix-web-mysql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent 		//apache
dnf install zabbix-server-mysql zabbix-web-mysql zabbix-nginx-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent 		//nginx
```

## 3.安装数据库

```bash
dnf install mariadb-server mariadb //安装数据库
mariadb-secure-installtion //设置数据库，10.3以上版本
```

## 4.创建初始数据库

```bash
show databases;
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'password';
grant all privileges on zabbix.* to zabbix@localhost;
flush privileges;
```

## 5.导入数据库

```bash
zcat /usr/share/doc/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uzabbix -p zabbix
```

## 6.修改zabbix server配置文件

/etc/zabbix/zabbix_server.conf

```bash
DBPassword=passwd
DBName=
DBUser=
DBSocket=/var/lib/mysql/mysql.scok
Server=IP
ListenPort=10050
```

## 7.修改时区

/etc/php-fpm.d/zabbix.conf

```bash
php_value[date.timezone] = Asia/Shanghai
```

## 7.配置PHP在nginx

/etc/nginx/conf.d/zabbix.conf

```bash
listen 80;
server_name example.com;
```

## 8.启动服务

```bash
systemctl start zabbix-server zabbix-agent httpd php-fpm
systemctl enable zabbix-server zabbix-agent httpd php-fpm
```
