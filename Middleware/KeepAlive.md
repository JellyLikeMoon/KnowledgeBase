# KeepAlived 安装

```bash
wget https://www.keepalived.org/software/keepalived-2.2.4.tar.gz
tar -xzvf keepalived-2.2.4.tar.gz
```

```bash
mkdir /usr/local/keepalived
mkdir /etc/keepalived
cd keepalived-2.2.4
./configure --prefix=/usr/local/keepalived
make && make install

```

# KeepAlived 主备配置

主节点:

编辑`/etc/keepalived/keepalived.conf`
```bash
global_defs { 
     router_id acp_zabbix_server                    # 负载均衡标识，在局域网内应该是唯一的
}

vrrp_script check_zabbix {                               # 配置虚拟脚本
   script "/etc/keepalived/check_zabbix.sh"              # 执行脚本,检查 zabbix 服务是否存活
    interval 3                                           # 脚本执行间隔:秒
}

# vrrp_instance 
vrrp_instance v_zabbix { 
   state MASTER                                # 指定该 keepalived 节点的初始状态（MASTER|BACKUP）  
   interface eth0                               # VRRP 实例绑定的网口，用于发送 VRRP 包
   virtual_router_id 200                    # 路由 ID，范围是 0-255，主备都一样
   priority 100                                   # 指定优先级，优先级高的将成为 MASTER
   advert_int 1                                  # 指定发送 VRRP 广播的间隔。单位是秒
authentication {                               # 身份验证
   auth_type PASS                          # 指定认证方式
  auth_pass 123456                       # 指定认证所使用的密码 ，主备都一样
}

track_script {                               # 调用"vrrp_script"的脚本
    check_zabbix                           # 增加一个跟踪脚本到网口上
}

virtual_ipaddress {                     # 虚拟 IP
    11.8.38.208/24 
    } 
}

```

设置主zabbix健康检查脚本

编辑`/etc/keepalived/check_zabbix.sh`
```bash
#!/bin/bash
#检查 Zabbix Server 服务
systemctl status zabbix-server &>/dev/null
if [ ? -ne 0 ];then
    echo -e "`date "+%F  %H:%M:%S"` 主机名： `hostname` Zabbix Server 服务异常，keepalived 切换" >> /usr/local/zabbix/logs/check_zabbix.log
    exit 1
else
   #检查 Zabbix Web 服务
   systemctl status httpd &>/dev/null
   if [? -ne 0 ];then
        echo -e "`date "+%F  %H:%M:%S"` 主机名： `hostname` Zabbix Web 服务异常，keepalived 切换" >> /usr/local/zabbix/logs/check_zabbix.log
        exit 1
   else
      exit 0
   fi
exit 0
fi
```

备节点:

编辑`/etc/keepalived/keepalived.conf`
```bash
global_defs { 
      router_id acp_zabbix_server                # 负载均衡标识，在局域网内应该是唯一的
}

# vrrp_instance 
vrrp_instance v_zabbix { 
   state BACKUP                                 # 指定该 keepalived 节点的初始状态（MASTER|BACKUP）
   interface eth0                                        # VRRP 实例绑定的网口，用于发送 VRRP 包
   virtual_router_id 200                                   # 路由ID，范围是0-255，主备都一样
   priority 90                                             # 指定优先级，优先级高的将成为 MASTER
   advert_int 1                                            # 指定发送VRRP广播的间隔。单位是秒

authentication {                                     # 身份验证
   auth_type PASS                                          # 指定认证方式
   auth_pass 123456                                       # 指定认证所使用的密码 ，主备都一样
}

notify_master /etc/keepalived/notify_zabbix.sh  # 转换成 master 时，执行的脚本
virtual_ipaddress { 
    11.8.38.208/24 
   } 
}

```

编辑 `/etc/keepalived/notify_zabbix.sh`
```bash
#!/bin/bash
echo -e "`date "+%F  %H:%M:%S"` Zabbix 发生主备切换" >> /usr/local/zabbix/logs/notify_zabbix.log

```

# KeepAlived 双主模式

```bash
global_defs { 
      router_id acp_zabbix_server                # 负载均衡标识，在局域网内应该是唯一的
}

# vrrp_instance 
vrrp_instance v_zabbix { 
   state BACKUP                                 # 指定该 keepalived 节点的初始状态（MASTER|BACKUP）
   interface eth0                                        # VRRP 实例绑定的网口，用于发送 VRRP 包
   virtual_router_id 200                                   # 路由ID，范围是0-255，主备都一样
   priority 90                                             # 指定优先级，优先级高的将成为 MASTER
   advert_int 1                                            # 指定发送VRRP广播的间隔。单位是秒

authentication {                                     # 身份验证
   auth_type PASS                                          # 指定认证方式
   auth_pass 123456                                       # 指定认证所使用的密码 ，主备都一样
}

notify_master /etc/keepalived/notify_zabbix.sh  # 转换成 master 时，执行的脚本
virtual_ipaddress { 
    11.8.38.208/24 
   } 
}

vrrp_instance a_zabbix { 
   state master                        # 指定该 keepalived 节点的初始状态（MASTER|BACKUP）
   interface eth0                                        # VRRP 实例绑定的网口，用于发送 VRRP 包
   virtual_router_id 201                                   # 路由ID，范围是0-255，主备都一样
   priority 90                                             # 指定优先级，优先级高的将成为 MASTER
   advert_int 1                                            # 指定发送VRRP广播的间隔。单位是秒

authentication {                                     # 身份验证
   auth_type PASS                                          # 指定认证方式
   auth_pass 123456                                       # 指定认证所使用的密码 ，主备都一样
}

notify_master /etc/keepalived/notify_zabbix.sh  # 转换成 master 时，执行的脚本
virtual_ipaddress { 
    11.8.38.209/24 
   } 
}

```
