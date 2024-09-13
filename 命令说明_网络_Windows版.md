### ping

```powershell
ping -c             //发送数据包的次数
ping -4/-6          //使用IPv4/IPv6
ping -s             //改变ICMP数据包的大小，默认64字节，"56"+8
ping -D             //打印当前unix时间戳
ping -i             //两次ping之间的时间间隔
ping -t             //持续ping，Windows持续ping需要此选项
```

### ipconfig

```powershell
> ipconfig                       ... 显示信息
> ipconfig /all                  ... 显示详细信息
> ipconfig /renew                ... 更新所有适配器
> ipconfig /renew EL*            ... 更新所有名称以 EL 开头的连接
> ipconfig /release *Con*        ... 释放所有匹配的连接，例如“有线以太网连接 1”或“有线以太网连接 2”
> ipconfig /allcompartments      ... 显示有关所有隔离舱的信息
> ipconfig /allcompartments /all ... 显示有关所有隔离舱的详细信息

/?               显示此帮助消息
/all             显示完整配置信息。
/release         释放指定适配器的 IPv4 地址。
/release6        释放指定适配器的 IPv6 地址。
/renew           更新指定适配器的 IPv4 地址。
/renew6          更新指定适配器的 IPv6 地址。
/flushdns        清除 DNS 解析程序缓存。
/registerdns     刷新所有 DHCP 租用并重新注册 DNS 名称
/displaydns      显示 DNS 解析程序缓存的内容。
/showclassid     显示适配器允许的所有 DHCP 类 ID。
/setclassid      修改 DHCP 类 ID。
/showclassid6    显示适配器允许的所有 IPv6 DHCP 类 ID。
/setclassid6     修改 IPv6 DHCP 类 ID。
```

### nslookup

```powershell
nslookup [-opt ...]             	# 使用默认服务器的交互模式
nslookup [-opt ...] - server    	# 使用 "server" 的交互模式
nslookup [-opt ...] host        	# 仅查找使用默认服务器的 "host"
nslookup [-opt ...] host server 	# 仅查找使用 "server" 的 "host"
```

### route

```powershell
> route PRINT
> route PRINT -4
> route PRINT -6
> route PRINT 157*													只打印那些匹配  157* 的项
> route ADD 157.0.0.0 MASK 255.0.0.0  157.55.80.1 METRIC 3 IF 2 	如果未给出 IF，它将尝试查找给定网关的最佳接口。
> route ADD 3ffe::/32 3ffe::1
> route CHANGE 157.0.0.0 MASK 255.0.0.0 157.55.80.5 METRIC 2 IF 2 	CHANGE 只用于修改网关和/或跃点数。
> route DELETE 157.0.0.0
> route DELETE 3ffe::/32  

  -f           清除所有网关项的路由表。如果与某个
               命令结合使用，在运行该命令前，
               应清除路由表。
  -p           与 ADD 命令结合使用时，将路由设置为
               在系统引导期间保持不变。默认情况下，重新启动系统时，
               不保存路由。忽略所有其他命令，
               这始终会影响相应的永久路由。
  -4           强制使用 IPv4。
  -6           强制使用 IPv6。
  command      其中之一:
                 PRINT     打印路由
                 ADD       添加路由
                 DELETE    删除路由
                 CHANGE    修改现有路由
  destination  指定主机。
  MASK         指定下一个参数为“netmask”值。
  netmask      指定此路由项的子网掩码值。
               如果未指定，其默认设置为 255.255.255.255。
  gateway      指定网关。
  interface    指定路由的接口号码。
  METRIC       指定跃点数，例如目标的成本。
```

### dig

```powershell
```

### Windows补丁手动安装

```powershell
expand -F:* C:\KB2719033.msu C:\update                                                 # 解压msu文件

dism.exe /online /add-package /packagepath:"C:\update\Windows6.1-KB2719003-x64.cab"    # 安装cab文件
```

‍

‍
