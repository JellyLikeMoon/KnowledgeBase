 # 设置静态IP

```bash
nmcli connection modify eth0 ipv4.addresses 192.168.1.2/24
nmcli connection modify eth0 ipv4.gateway 192.168.1.1
nmcli connection modify eth0 ipv4.dns 8.8.8.8
nmcli connection modify eth0 ipv4.method manual                                                                                 # 设置为静态
nmcil connection up eth0                                                                                                        # 开启接口
nmcil connection down eth0                                                                                                      # 关闭接口
nmcil connection up "lan_eth0"                                                                                                  # 开启配置
nmcli connection reload                                                                                                         # 重新加载
nmcli connection modify eth0  ip4 192.168.0.1/24 gw4 192.168.0.254 ipv4.dns 8.8.8.8 ipv4.method manual autoconnect yes          # 快速配置
nmcli connection modify "Wired connection 1" connection.id enp1s0                                                               # 修改默认配置名
nmcli connection add con-name configname type ethernet ifname eth0                                                              # 添加配置文件
nmcli connection add con-name static ifname eth0 type ethernet ip4 192.168.0.1/24 gw4 192.168.0.254 ipv4.dns 8.8.8.8 ipv4.method manual autoconnect no  # 添加配置文件
nmcli connection up static
```

# 添加vlan标签, con-name配置文件, ifname创建虚拟网卡, dev依附于物理网卡

```bash
nmcli connection add con-name vlan10 ifname eth0-vlan0 type vlan id 100 dev eth0 ipv4.address 1.1.1.1/24 ipv4.method manual

```

# 添加链路聚合

```bash
nmcli connection add con-name bond0 ifname bond0 type bond mode active-backup ipv4.address 1.1.1.1/24 ipv4.method manual

nmcli connection add con-name eth0-bond0 ifname eth1 type bond-slave master bond0
nmcli connection add con-name eth0-bond1 ifname eth2 type bond-slave master bond0

```


 # 关闭开启nmcli服务

```bash
nmcli networking
nmcli networking on
nmcli networking off
```

 # 查看网卡信息

```bash
ethtool eth0
nmcli connection show               # 查看网卡列表
nmcli connection show -active       # 查看活跃网卡列表
nmcli connection show eth0          # 查看网卡信息
nmcli device show eth0              # 查看网卡物理参数
```

 # 查看网卡状态

```bash
nmcli device status
nmcli general                       #  验证是否可以通信
```

 # 修改网卡配置文件

`/etc/sysconfig/network-scripts/ifcfg-eth0`
