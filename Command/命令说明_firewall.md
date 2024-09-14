### 官网地址

[https://firewalld.org/]()

### 安装

```bash
dnf install firewalld
```

### 命令

```bash
firewall-cmd --state                                        //查看状态
firewall-cmd --list-all                                     //打印所有信息
firewall-cmd --list-ports                                   //打印所有端口
firewall-cmd --list-services                                //打印所有服务
firewall-cmd --reload                                       //重载
firewall-cmd --get-zones                                    //获取所有区域
firewall-cmd --get-default-zone                             //查看默认区域
firewall-cmd --zone=public --list-all                       //指定查看区域配置
firewall-cmd --get-active-zones                             //查看使用的区域
firewall-cmd --list-all-zones                               //查询当前所属区域
firewall-cmd --zone=work --change-interface=eth0            //修改接口区域
firewall-cmd --set-default-zone=work                        //修改默认区域
firewall-cmd --get-services                                 // "/usr/lib/firewalld/services" 查看相关服务信息
```

### 配置文件

/usr/lib/firewalld
/etc/firewalld

### 自定义策略

```bash
firewall-cmd --permanent --add-port=80/tcp --zone=public
firewall-cmd --permanent --remove-port=80/tcp --zone=public
firewall-cmd --permanent --add-port=80-90/tcp --zone=public

firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --remove-service=http

firewall-cmd --permanent --add-rich-rul ' rule family=ipv4 source address=10.0.0.1 port protocol=tcp port=3306  accept '          //指定 ip 访问指定协议端口
firewall-cmd --permanent --add-rich-rul ' rule family=ipv4 source address=10.0.0.1 port protocol=tcp port=3306  reject'           //指定 ip 禁止访问指定协议端口
firewall-cmd --permanent --add-rich-rul ' rule family=ipv4 source address=10.0.0.1 accept '                                       //允许 IP 访问
firewall-cmd --permanent --add-rich-rul ' rule family=ipv4 source address=10.0.0.1 drop '                                         //禁止 IP 访问
firewall-cmd --permanent --add-rich-rule 'rule family=ipv4 source address=10.0.0.1/24 accept'                                     //允许 IP 段访问
firewall-cmd --permanent --remove-rich-rule 'rule family=ipv4 source address=10.0.0.1 port protocol=tcp port=3306  accept'        //移除规则
```

### 端口转发

```bash
firewall-cmd --zone=external --add-masquerade                                                                           //启用 external 区域伪装
firewall-cmd --zone=external --add-forward-port=port=80:proto=tcp:toport=8080                                           //同计算机端口转发
firewall-cmd --zone=external --add-forward-port=port=80:proto=tcp:toaddr=10.10.10.2                                     //不同计算机端口转发
firewall-cmd --zone=external --add-forward-port=port=80:proto=tcp:toport=8080:toaddr=10.10.10.2                         //不同计算机端口转发
```

### 区域说明

```bash
drop-删除所有传入连接，而无任何通知。仅允许传出连接。
block-对于 IPv6 IPv4 和 icmp6-adm-prohibited/IPv6n，所有传入连接均被拒绝，并带有 icmp-host-prohibited 消息。仅允许传出连接。
public-用于不受信任的公共区域。即您的计算机在一个不可信的计算机网络上，但你可以指定那些连接允许传入。
external-对于用在内部网络。当你的系统充当网关或者路由时，在网络上的计算机通常是可信的，仅已允许连接可以传入。
internal-对于在内部网络上。当计算机充当网关或路由器时。网络上的其它计算机通常是受信任的。仅已允许连接可以传入。
dmz-对于计算机位于可缓冲区，区域内计算机访问网络时将会被限制。仅已允许连接可以传入。
work-用于工作机。网络上的其他计算机通常是受信任的。仅已允许连接可以传入。
home-用于家庭计算机。网络上的其他计算机通常是受信任的。仅已允许连接可以传入。
trusted-接受所有网络连接。信任在网络中的所有计算机。
```
