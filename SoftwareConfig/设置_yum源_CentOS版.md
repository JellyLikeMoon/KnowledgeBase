---
title: 设置_yum源_CentOS版
date: 2024-06-11T22:52:10Z
lastmod: 2024-06-11T22:52:10Z
---

### 安装依赖

```bash
dnf install yum-utils createrepo
```

### 同步至本

```bash
reposync -p /root/zabbix  --repoid=zabbix --download-metadata
reposync -r epel -p /root
```

### 制作repodata

```bash
createrepo -pdo /root/zabbix
createrepo --update /root/zabbix 						//更新
createrepo -v /root/zabbix
```

### 永久挂载

`/etc/fstab`

```bash
/dev/sr0    /var/www/html/centos    iso9660     defaults    0     0
```

### 临时挂载

```bash
mount -o -loop /root/centos.iso /media
cp -r /media /var/www/html/iso
```

### 制作ISO镜像

```bash
mkisofs -r -o /root/centos.iso /var/www/html/centos
```

### yum优先级，安装插件

```bash
rpm -q yum-plugin*
```

### repo文件添加优先级

`/etc/yum.repo.d/centos.repo`

```bash
priority=1 //优先级1，2，3...
```

‍
