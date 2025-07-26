- [Docker 官网](#docker-官网)
- [Docker 安装](#docker-安装)
  - [自动安装](#自动安装)
  - [手动安装](#手动安装)
- [Docker 镜像源](#docker-镜像源)
- [docker-compose 安装](#docker-compose-安装)
- [命令](#命令)
- [命令示例](#命令示例)
- [Dockerfile](#dockerfile)
- [DockerCompose](#dockercompose)

# Docker 官网

Docker官网: [https://www.docker.com/](https://www.docker.com/)
Docker安装文档: [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/ "docker")
Docker-Compose安装文档: [https://docs.docker.com/compose/install/linux/](https://docs.docker.com/compose/install/linux/)

# Docker 安装

## 自动安装

```bash
# 指定安装源
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun && sudo systemctl enable docker && sudo systemctl start docker && docker version

# 默认docker官网安装
curl -fsSL https://get.docker.com | bash -s docker && sudo systemctl enable docker && sudo systemctl start docker && docker version

# 加速镜像地址
touch /etc/docker/daemon.json
echo "{\"registry-mirrors\": [\"https:\/\/docker.mirrors.ustc.edu.cn\"]}" > /etc/docker/daemon.json

sudo systemctl daemon-reload
sudo systemctl restart docker

sudo docker run hello-world
```

## 手动安装

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

# Docker 镜像源

​`/etc/docker/daemon.json`  
已失效
```bash
https://mirror.baidubce.com
https://docker.mirrors.sjtug.sjtu.edu.cn
https://docker.nju.edu.cn
https://docker.mirrors.ustc.edu.cn
```

# docker-compose 安装

```bash
sudo apt-get update
sudo apt-get install docker-compose-plugin          # docker-compose以docker插件的方式安装
sudo apt install docker-compose                     # docker-compose以apt软件包的方式安装
```

# 命令

```bash
Common Commands:
  run         Create and run a new container from an image
  exec        Execute a command in a running container
  ps          List containers
  build       Build an image from a Dockerfile
  pull        Download an image from a registry
  push        Upload an image to a registry
  images      List images
  login       Authenticate to a registry
  logout      Log out from a registry
  search      Search Docker Hub for images
  version     Show the Docker version information
  info        Display system-wide information

Management Commands:
  builder     Manage builds
  buildx*     Docker Buildx
  compose*    Docker Compose
  container   Manage containers
  context     Manage contexts
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  plugin      Manage plugins
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Swarm Commands:
  swarm       Manage Swarm

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  import      Import the contents from a tarball to create a filesystem image
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  wait        Block until one or more containers stop, then print their exit codes

Global Options:
      --config string      Location of client config files (default "/root/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and
                           default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket to connect to
  -l, --log-level string   Set the logging level ("debug", "info", "warn", "error", "fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/root/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/root/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/root/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

```

# 命令示例


# Dockerfile


# DockerCompose

