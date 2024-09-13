---
title: 命令说明_apt
date: 2024-06-11T22:52:20Z
lastmod: 2024-06-11T22:52:20Z
---

 # 官网地址

> [https://ubuntu.com/server/docs/package-management]()

 # 命令

```bash
  list                  # 列出所有安装包，列出所有已安装软件包版本"--all-version"
  search                # 根据命令描述搜索软件包
  show                  # 列出软件包的详细信息
  install               # 安装软件包，安装指定版本"apt=2.4.5"，仅下载不安装“-d”
  reinstall             # 重新安装软件包，仅下载不安装“-d”
  remove                # 移除软件包
  autoremove            # 自动删除未被依赖的软件包
  update                # 更新源
  upgrade               # 更新所有软件包
  full-upgrade          # 先删除需要更新的安装包再重新安装该安装包
  purge                 # 移除安装包及配置文件
  edit-sources          # 编辑源文件“source.list”
  satisfy               # 安装该软件包并下载所有依赖包
  clean                 # 删除下载的软件包
  source                # 获取包源文件
  build-dep             # 从源文件构建软件包或删除软件包
  autoclean             # 仅删除不再下载的软件包
  download              # 将二进制文件下载到当前目录
  check                 # 检查是否存在损坏依赖
```

 # 选项

```bash
-h                 # 本帮助文件。
-q                 # 输出到日志 - 无进展指示
-qq                # 不输出信息，错误除外
-d                 # 仅下载及依赖 - 不安装或解压归档文件
-s                 # 不实际安装。模拟执行命令
-y                 # 假定对所有的询问选是，不提示
-f                 # 尝试修正系统依赖损坏处
-m                 # 如果归档无法定位，尝试继续
-u                 # 同时显示更新软件包的列表
-b                 # 获取源码包后编译
-V                 # 显示详细的版本号
```

 # dpkg

```bash
dpkg -i package                # 安装包
dpkg -r package                # 移除安装包
dpkg -L package                # 列出包安装的所有文件清单
dpkg -P package                # 完全删除包，包括配置文件
dpkg -s package                # 显示包的信息
dpkg -c package                # 查看包的内容
dpkg -I package                # 提取包的信息
```
