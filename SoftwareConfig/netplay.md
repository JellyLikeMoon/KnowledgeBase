# 配置文件目录

`/etc/netplan/00-install-config.yaml`

```bash
network:
    version: 2
    renderer: networkd
    ethernets:
        ens33:
            addresses:
                - 192.168.1.247/24
            nameservers:
                addresses: [4.2.2.2, 8.8.8.8]
            routes:
                - to: default
                  via: 192.168.1.1
            dhcp4: false
            dhcp6: false
```

# 重启服务

```bash
sudo netplan apply
```

# 查看网卡 IP

```bash
ip addr show ens33
```

# 查看默认路由

```bash
ip route show
```

‍

‍

‍

‍

‍
