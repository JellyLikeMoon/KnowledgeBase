---
title: 部署_ProxmoxVE8
date: 2024-06-11T22:52:18Z
lastmod: 2024-06-11T22:52:18Z
---

### 国内镜像

[http://mirrors.ustc.edu.cn/proxmox/](http://mirrors.ustc.edu.cn/proxmox/)  
[http://download.proxmox.wiki](http://download.proxmox.wiki)

### 查看debian版本

```bash
cat /etc/debian_version
cat /etc/issue
lsb_release -a
```

### 删除企业源

```bash
rm -rf /etc/apt/sources.list.d/pve-enterprise.list
```

### 添加社区源

```bash
echo "deb http://mirrors.ustc.edu.cn/proxmox/debian/pve bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list
```

### 替换国内debian源

```bash
## 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
## deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
## deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
## deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware

## deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware
## deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware

deb https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
## deb-src https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
```
