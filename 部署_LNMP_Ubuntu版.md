---
title: 部署_LNMP_Ubuntu版
date: 2024-06-11T22:51:54Z
lastmod: 2024-06-11T22:51:54Z
---

# 1. 替换清华源

```bash
sudo sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
```

```bash
sudo apt update && sudo apt upgrade
```

# 2. 安装 Nginx

```bash
sudo apt install nginx
nginx -v
sudo systemctl enable nginx && sudo systemctl status nginx
```

# 3. 安装默认源 php 版本

```bash
sudo apt install php-fpm php-cli / apt install php php-fpm / apt install php7.4-cli php7.4-fpm
php -v
sudo apt install php8.1-curl php8.1-redis 														//按需安装对应版本的扩展php*-*
sudo systemctl enable php*-fpm 																	//对应PHP版本的php*-fpm
```

# 4. 安装其他 PHP 版本

```bash
sudo add-apt-repository
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php7.4-cli php7.4-fpm
```

# 5. php 与 nginx 关联

/etc/nginx/sites-enabled/default.conf

```bash
location ~ \.php${
	fastcgi_pass unix:/run/php/php8.1-fpm.sock; 	//取消注释此行
}
```

Nginx 测试

```bash
nginx -s reload
echo "<?php phpinfo(); ?>" > /var/www/html/index.php
curl http://localhost:80
```

# 6. 安装数据库 mariaDB

```bash
sudo apt install mariadb-server mariadb-client
sudo netstat -tap | grep mariadb
mysql_secure_installation //mariaDB10.8版本使用mariadb-secure-installation
sudo systemctl enable mariadb && sudo systemctl status mariadb
```

* 配置文件

/etc/mysql/mariadb.conf.d/50-server.cnf

```bash
bind-address = 127.0.0.1 						//注释此行，目的是允许远程连接或者更改为0.0.0.0
```

```bash
sudo systemctl restart mariadb
```

* 数据库脚本

```sql
create database DB;
use DB;
CREATE TABLE `students` (
	`student_id` INT(11) NOT NULL AUTO_INCREMENT,
		`student_name` VARCHAR(100) NOT NULL,
	`student_address` VARCHAR(40) NOT NULL,
	`admission_date` DATE NULL DEFAULT NULL,
	PRIMARY KEY (`student_id`)
)COLLATE='utf8_general_ci' ENGINE=InnoDB;

create table if not exists test (
	id bigint auto_increment primary key,
	name varchar(128) charset utf8,
	key name (name(32))
) engine=InnoDB default charset utf8;
```

* 数据库导入导出

```bash
mysqldump -uroot -p db > /home/db.sql 			//同时导出数据
mysqldump -uroot -p -d db > /home/db.sql 		//只导出表结构
```

```sql
create database xxx;
use xxx;
set names utf8;
source /home/sb.sql							//导入数据库

mysql -u root -p xxx < /home/db.sql			//导入数据库
```

* 数据库设置用户及权限

```sql
create user ''@'' identified by '';
grant all privileges on *.* to ''@'';
flush privileges;

select user,host,plugin from mysql.user;
```


