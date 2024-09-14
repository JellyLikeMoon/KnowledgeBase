### 解压

```bash
tar -zxvf binaries.tar.gz
mv ./binaries/Linux/intel/static/vlmcsd-x64-musl-static /usr/kms/vlmcsd
```

### 配置服务

/usr/lib/systemd/system/kms.service

内容如下：

```bash
[Unit]
Description=vlmcsd - service
After=network.target

[Service]
Type=forking
ExecStart=/usr/kms/vlmcsd
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID

[Install]
WantedBy=multi-user.target
```

### 重载生效

```bash
systemctl daemon-reload
```

### 常用命令

```bash
systemctl start kms
systemctl enable kms
systemctl disable kms
```

### 状态查询

```bash
netstat -lnp | grep 1688
```

### 开启防火墙

```bash
systemctl start firewalld
firewall-cmd --list-services
firewall-cmd --zone=public --list-ports
firewall-cmd --reload
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone= public --remove-port=80/tcp --permanent
```
