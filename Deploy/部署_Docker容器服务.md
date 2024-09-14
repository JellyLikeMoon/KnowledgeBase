- ### NAS系统

> https://github.com/IceWhaleTech/CasaOS

### 安装

```bash
wget -qO- https://get.casaos.io | sudo bash

curl -fsSL https://get.casaos.io | sudo bash
```

### 卸载

```bash
casaos-uninstall

curl -fsSL https://get.icewhale.io/casaos-uninstall.sh | sudo bash
```

- ### Web监控系统

> https://github.com/louislam/uptime-kuma

### 安装

```bash
mkdir -p /data/uptime-kuma
docker run -d --restart=always -p 3001:3001 -v /data/uptime-kuma:/app/data --name uptime-kuma louislam/uptime-kuma:1
```

* ### Web小说

hectorqin/reader

```bash
# 自行编译
# docker build -t reader:latest .

# 使用环境变量覆盖服务配置，环境变量采用大写字母，不允许使用.-符号，采用下划线“_”取代点“.”  减号“-”直接删除

# docker run -d --restart=always --name=reader -e "SPRING_PROFILES_ACTIVE=prod" -v $(pwd)/logs:/logs -v $(pwd)/storage:/storage -p 8080:8080 reader:latest

# 跨平台镜像

# 新建构建器
# docker buildx create --use --name mybuilder
# 启动构建器
# docker buildx inspect mybuilder --bootstrap
# 查看构建器及其所支持的cpu架构
# docker buildx ls
# 构建跨平台镜像
# docker buildx build -t reader:latest --platform=linux/arm,linux/arm64,linux/amd64 . --push

# 使用预编译的镜像

# 自用版(建议修改映射端口)
docker run -d --restart=always --name=reader -e "SPRING_PROFILES_ACTIVE=prod" -v $(pwd)/logs:/logs -v $(pwd)/storage:/storage -p 8080:8080 hectorqin/reader

# 多用户版(建议修改映射端口)
docker run -d --restart=always --name=reader -v $(pwd)/logs:/logs -v $(pwd)/storage:/storage -p 8080:8080 hectorqin/reader java -jar /app/bin/reader.jar --spring.profiles.active=prod --reader.app.secure=true --reader.app.secureKey=管理密码 --reader.app.inviteCode=注册邀请码

# 多用户版 使用环境变量(建议修改映射端口)
docker run -d --restart=always --name=reader -e "SPRING_PROFILES_ACTIVE=prod" -e "READER_APP_SECURE=true" -e "READER_APP_SECUREKEY=管理密码" -e "READER_APP_INVITECODE=注册邀请码" -v $(pwd)/logs:/logs -v $(pwd)/storage:/storage -p 8080:8080 hectorqin/reader

# 更新docker镜像
# docker pull hectorqin/reader

#:后面的端口修改为映射端口
# web端 http://localhost:8080/
# 接口地址 http://localhost:8080/reader3/

# 通过watchtower手动更新
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --cleanup --run-once reader

# 使用 remote-webview 功能
# 1.创建 remote-webview 容器
docker run -d --network host --restart=always hectorqin/remote-webview
# 2.重建 reader 容器
reader使用宿主机网络：--network host
reader添加环境变量：-e "READER_APP_REMOTEWEBVIEWAPI=http://localhost:8050"
获取reader添加参数：--reader.app.remoteWebviewApi=http://localhost:8050"
```
