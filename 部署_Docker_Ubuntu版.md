- [Docker官网](#docker官网)
- [Docker安装文档](#docker安装文档)
- [Docker-Compose安装文档](#docker-compose安装文档)
- [安装](#安装)
    - [自动安装](#自动安装)
    - [手动安装](#手动安装)
- [Docker镜像源](#docker镜像源)
- [安装docker-compose](#安装docker-compose)

---

# Docker官网

[https://www.docker.com/](https://www.docker.com/)

# Docker安装文档

[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/ "docker")

# Docker-Compose安装文档

[https://docs.docker.com/compose/install/linux/](https://docs.docker.com/compose/install/linux/)

# 安装

### 自动安装

```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun && sudo systemctl enable docker && sudo systemctl start docker && docker version

curl -fsSL https://get.docker.com | bash -s docker && sudo systemctl enable docker && sudo systemctl start docker && docker version

touch /etc/docker/daemon.json
echo "{\"registry-mirrors\": [\"https:\/\/docker.mirrors.ustc.edu.cn\"]}" > /etc/docker/daemon.json

sudo systemctl daemon-reload
sudo systemctl restart docker

sudo docker run hello-world
```

### 手动安装

- 安装依赖
```bash
apt-get update
apt-get install ca-certificates curl gnupg
```
- GPG公钥
```bash
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
- 添加镜像源
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
```
- 安装Docker
```bash
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

# Docker镜像源

​`/etc/docker/daemon.json`  
已失效
```bash
https://mirror.baidubce.com
https://docker.mirrors.sjtug.sjtu.edu.cn
https://docker.nju.edu.cn
https://docker.mirrors.ustc.edu.cn
```

# 安装docker-compose

```bash
sudo apt-get update
sudo apt-get install docker-compose-plugin          # docker-compose以docker插件的方式安装
sudo apt install docker-compose                     # docker-compose以apt软件包的方式安装
```
