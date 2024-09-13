### 官网地址

[https://fedoraproject.org/wiki/DNF/zh-cn]()

[https://dnf.readthedocs.io/en/latest/index.html]()

### 命令

```bash
dnf upgrade                                       # 更新所有可更新的软件包
dnf update                                        # 更新所有源
dnf update package                                # 指定更新软件包
dnf check-update                                  # 检查系统软件包的更新
dnf disro-sync                                    # 更新所有软件包至最新稳定版
dnf install
dnf install -enablerepo=epel package              # 指定软件仓库安装软件包
dnf reinstall                                     # 重新安装
dnf remove                                        # 删除软件包
dnf autoremove                                    # 不需要的依赖包
dnf remove --duplicates                           # 删除重复包
dnf list                                          # 列出来自仓库的所有软件列表及已安装软件包
dnf clean all                                     # 删除缓存的无用安装包
dnf list available                                # 列出软件库可供安装的软件包
dnf list --duplicates                             # 列出软件所有可用版本
dnf grouplist                                     # 列出所有软件包组
dnf groupinstall 'software'                       # 安装软件包组
dnf install @design-suite                         # 安装软件包组design-suite
dnf groupupdate 'software'                        # 更新软件包组
dnf groupremove                                   # 删除软件包组
dnf search package                                # 搜索软件包
dnf provides                                      # 查找某一文件或命令的软件包
dnf info package                                  # 列出软件包信息
dnf -h / dnf install -h                           # 帮助信息
dnf help                                          # 帮助信息
dnf -version                                      # 查看DNF版本
dnf downgrade package                             # 回滚至特定版本
dnf repolist                                      # 查看可用的DNF源
dnf repolist all                                  # 可用及不可用的DNF源
dnf repoquery --list package                      # 查看软件仓库中有关软件包的元数据并列出该软件提供的所有文件
dnf repoquery --deplist package                   # 列出软件包及其所有依赖包
dnf download package                              # 下载软件包但不安装
dnf download --resolve package                    # 下载软件包但不安装且不包括依赖包
dnf download --resolve --alldeps package          # 下载软件及其所有依赖包（包含当前系统已安装的），但不安装
```
