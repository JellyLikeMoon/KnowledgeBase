- [安装MariaDB](#安装mariadb)
- [安装PHP](#安装php)
- [安装Nginx](#安装nginx)
- [安装composer](#安装composer)
- [安装chemex](#安装chemex)

---

## 安装MariaDB

```bash
apt install mariadb-server

mysql_secure_installation

mysql -uroot -p
mysql>    create database chemex;
create user 'chemex'@'%' identified by 'chemex';
grant all privileges on chemex.* to 'chemex'@'%' with grant option;
flush privileges;
```

## 安装PHP

```bash
apt-get install software-properties-common apt-transport-https lsb-core ca-certificates php8.1-mbstring php8.1-bz2 php8.1-bcmath php8.1-soap php8.1-gd php8.1-mysql php8.1-xmlrpc php8.1-dev php8.1-curl php8.1-xml php8.1-xsl php8.1-zip php8.1-fpm php8.1-gmp php8.1-cli php8.1-opcache php8.1-ldap

php -v
```

## 安装Nginx

```bash
apt install nginx
```

vi /etc/nginx/sites-enabled/default

```bash
server {
root /vat/www/html/chemex/public/;
index index.php index.html;
server_name 127.0.0.1;

location / {

## root /vat/www/html/chemex/public/;

## index index.php index.html;

try_files $uri$uri/ /index.php?$args; //添加
}

location ~ \.php$ {
include snippets/fastcgi-php.conf;
include fastcgi_params;
fastcgi_pass unix:/run/php/php8.1-fpm.sock;
}
}
```

```bash
systemctl start nginx
```

## 安装composer

```bash
wget https://getcomposer.org/composer.phar
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
chmod +x composer

composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/           //全局更换阿里源
composer config repo.packagist composer https://mirrors.aliyun.com/composer/              //当前项目更换阿里源

composer clear                //清理缓存
composer --version
composer selfupdate           //更新composer
composer update --lock        //已经通过其他源更新，则更新composer.lock文件
```

## 安装chemex

```bash
git clone https://github.com/celaraze/chemex.git .          //github main分支没有vendor目录
cd /chemex
composer install -vvv                                       //安装vendor文件
chmod -R 755 chemex
chmod -R 755 bootstrap
chmod -R 755 storage
cp .env.example .env
```

vi .env

```bash
DB_CONNECTION=mysql #数据库类型，不需要修改（兼容mariadb）##
DB_HOST=127.0.0.1 ## 数据库地址
DB_PORT=3306 ## 数据库端口号
DB_DATABASE=chemex ## 数据库名称
DB_USERNAME=chemex ## 数据库用户名
DB_PASSWORD=chemex ## 数据库密码

```

命令操作

```bash
php artisan key:generate
php artisan chemex:install
php artsan chemex:update      //升级
composer update -vvv          //更新依赖包

http://localhost
```
