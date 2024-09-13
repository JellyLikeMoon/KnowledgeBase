- [1. 在线源](#1-在线源)
- [2. 离线源](#2-离线源)
- [3. 本地源](#3-本地源)
- [4. 本地仓库](#4-本地仓库)
- [5. 配置 Web](#5-配置-web)
- [6. apt 软件源](#6-apt-软件源)

---

# 1. 在线源

* 清华源 Ubuntu 22.04 LTS

```bash
sudo sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
```

* 配置文件

`‍/etc/apt/sources.list`

```bash
## 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
## deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
## deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
## deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
## deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

## 预发布软件源，不建议启用
## deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
## deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```

* 升级

```bash
apt-get update
apt install dpkg-dev build-essential //开发依赖包
apt upgrade
```

# 2. 离线源

1. 下载软件

   ```bash
   sudo apt-get -d install <packages>       				   	//该方式仅能下载系统未下载的软件及依赖，若已经下载则无法下载
   sudo apt-get -d --reinstall install <packages>  			//已重新安装的方式下载，仍可能无法下载依赖软件
   ```
2. 拷贝软件

   ```bash
   sudo mkdir ./offline-apt-packages
   sudo chmod -R 777 /offline-apt-packages
   sudo cp -r /var/cache/apt/archives/* /offline-apt-packages
   ```
3. 建立依赖关系

   ```bash
   sudo  apt-get install -y dpkg-dev
   cd /offline-apt-packages
   sudo dpkg-scanpackages . /dev/null | gzip -9c > Packages.gz
   sudo cp Packages.gz ./archives
   ```
4. 打包为离线包

   ```bash
   sudo tar czvf offline-apt-packages.tar.gz /offline-apt-packages/*
   ```
5. 离线源使用

   ```bash
   sudo tar zxvf offline-apt-packages.tar.gz -C /tmp

   # 方式一（推荐）
   sudo echo "deb [trusted=yes] file:///tmp/offline-apt-packages  archives/"| sudo tee /etc/apt/sources.list                    	//允许不安全源使用
   # 方式二
   apt-get update --allow-insecure-repositories && apt-get install -f && apt install <packages> --allow-unauthenticated      		//允许不安全源安装

   sudo  apt-get update && apt-get install <packages>
   ```

# 3. 本地源

```bash
mkdir /media/cdrom
mount -t iso9660 -o loop /dev/sr0 /media/cdrom
sudo apt-cdrom -m -d=/media/cdrom add
apt update
apt upgrade  
```

# 4. 本地仓库

* 安装源同步工具
```bash
apt-get install apt-mirror
```

* 配置文件

`/etc/apt/mirror.list`

```bash
set nthreads     20
set _tilde 0
set base_path /opt/mirrors/ubuntu
set defaultarch amd64

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse

## deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse

## deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

## deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

## deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
```

* 清理源

```bash
clean https://mirrors.tuna.tsinghua.edu.cn/ubuntu/
```

* 同步源

```bash
apt-mirror
```

# 5. 配置 Web

* 配置文件

/etc/nginx/sites-available/default

```bash
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	root /var/www/html;
	index index.html index.htm index.nginx-debian.html;
	server_name _;

	location / {
	try_files $uri $uri/ =404;
	autoindex on; //目录浏览功能
	autoindex_exact_size off;
	autoindex_localtime on;
	}
}
```

* 软连接

```bash
ln -s /opt/mirrors/ubuntu/mirror/mirrors.aliyun.com/ubuntu /var/www/html/ubuntu
```

* 重载

```bash
systemctl nginx reload
```

# 6. apt 软件源

* 通过 add-apt-repository 安装

```
apt install software-properties-common dirmngr apt-transport-https
```

* 添加公钥，Ubuntu22.04 已弃用 apt-key，使用/etc/apt/trusted.gpg.d

```bash
curl -o /etc/apt/trusted.gpg.d/mariadb_release_signing_key.asc 'https://mariadb.org/mariadb_release_signing_key.asc'
```

* 添加源

```bash
add-apt-repository 'deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/mariadb/repo/10.9.3/ubuntu/ jammy main'
```

* 删除源

```bash
add-apt-repository --remove 'deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/mariadb/repo/10.9.3/ubuntu/ jammy main'
```

* 通过 ppa 软件源安装

```
add-apt-repository ppa:jonathof/ffmpeg-4
```

* 手动安装

vi /etc/apt/sources.list

```bash
deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/mariadb/repo/10.9.3/ubuntu/ jammy main
```

* 添加公开源 key

```bash
# 方式一
wget https://mirrors.tuna.tsinghua.edu.cn/mariadb/repo/10.9.3/ubuntu/dists/jammy/Release.gpg && mv ./Release.gpg /etc/apt/trusted.gpg.d/
# 方式二
curl -o /etc/apt/trusted.gpg.d/mariadb_release_signing_key.asc 'https://mariadb.org/mariadb_release_signing_key.asc'
```
