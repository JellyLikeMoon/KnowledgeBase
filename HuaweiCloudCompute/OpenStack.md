# OpenStack 概述

通过虚拟化软件将硬件资源从基础操作系统中抽离出来, 通过 OpenStack 进行统一资源调度和管理, 上层的能力一切建立在基础操作系统之上

OpenStack 是云计算管理平台, 是系统的控制面, 不涉及虚拟化, 主要是提供对外的统一管理接口
Hypervisor, 存储和网络等组成数据面, 主要是通过虚拟化抽离出硬件资源, 隔离环境, 提供虚拟化特性

云计算由多种系统组成, OpenStack 只是其中的管理平台

- OpenStack 与与云计算关系图

![](/HuaweiCloudCompute/img/云计算组成.png)

# OpenStack 架构

- 架构图

![](/HuaweiCloudCompute/img/OpenStack架构图.png)

由 OpenStack, 生命周期管理(lifecycle management), 集成系统(integration enablers), 客户端工具(client tools), 操作工具(operations tooling)组成

- 生产环境部署示例

![](/HuaweiCloudCompute/img/OpenStack生产环境部署示例.png)

由多台控制节点(通常控制服务与计算服务分离), 多台计算节点, 多台存储节点组成

- OpenStack 服务内部: 由多个进程组成
  所有服务(Keystone 除外)都至少有一个 API 进程, 负责监听 API 请求, 对请求进行预处理并将它们传递给服务的其他
  部分

- OpenStack 服务的进程间: 使用 AMQP 消息代理通信
  服务的状态存储在数据库中. 在部署和配置 OpenStack 云时, 管理员可以在多种消息代理和数据库解
  决方案中进行选择, 例如 RabbitMQ、MySQL、MariaDB 和 SQLite

- OpenStack 服务之间: 使用 API 进行通信
  客户端, Web, 第三方集成都可以通过 API 访问 OpenStack 服务

# OpenStack 核心服务

- 核心服务

| 服务名称     |  组件名称  | 功能描述                                                                                                                 |
| :----------- | :--------: | :----------------------------------------------------------------------------------------------------------------------- |
| 页面管理服务 |  Horizon   | 提供基于 Web 的控制界面, 使管理员和用户可以管理 OpenStack 资源和服务                                                     |
| 认证服务     |  Keystone  | 提供身份认证服务, 提供分布式租户授权, 支持各种认证服务                                                                   |
| 计算服务     |    Nova    | 提供可扩展, 大规模, 按需服务的计算资源, 支持裸金属, 虚拟机和容器                                                         |
| 镜像服务     |   Glance   | 提供镜像服务, 提供发现, 注册和检索服务, 镜像可存放在不同地方, 如 swift, cinder 或文件系统                                |
| 网络服务     |  Neutron   | 提供网络服务, 负责管理虚拟网络, 专注于为 openstack 提供网络即服务                                                        |
| 块存储服务   |   Cinder   | 提供块存储服务, 为虚拟机提供持久化存储服务, 将各种存储设备转化为块存储                                                   |
| 对象存储服务 |   Swift    | 提供对象存储服务, 提供高效安全且廉价的存储大量数据, 适合存储需要弹性扩展的非结构化数据                                   |
| 编排服务     |    Heat    | 为云应用提供编排 openstack 基础架构资源(如 CPU,内存,存储,网络等)                                                         |
| 计量服务     | Ceilometer | 提供计量服务, 提供对 openstack 核心组件规范化和转换数据的能力, 为所有 openstack 核心组件提供客户计费, 资源跟踪和告警服务 |

- OpenStack VM 创建流程示例

![](/HuaweiCloudCompute/img/OpenStack创建VM交互示例.png)

Heat 使用 Horizon 编排运行 VM 所需的资源, Horizon 向各种资源发送调度请求, 并向 Keystone 获得身份认证和授权, 各类资源服务接收到 Horizon 的资源申请请求后向 Keystone 进行身份认证和授权, 获得身份认证和授权后进行资源调度, 并通过 nova 根据 Heat 的内容调度虚拟化底座进行计算资源调配并运行 VM

# 页面管理服务 Horizon

- 定位

Horizon 属于全局组件之一, 提供一个 Web 管理界面, 与其他 openstack 服务交互, 管理员通过 Horizon 实现资源管理, 实例生命周期管理等等

- 作用

- 交互关系

Horizon 唯一依赖的服务是 Keystone

- 架构

![](/HuaweiCloudCompute/img/Horizon-体系架构.png)

# 认证服务 Keystone

- 定位

Keystone 是 OpenStack 的默认身份管理系统, Keystone 属于共享服务层, 为其他 OpenStack 服务提供认证

- Keystone 基本概念

|    名称    | 含义                                                                                            |
| :--------: | :---------------------------------------------------------------------------------------------- |
|   Domain   | 域, Keystone 中一个虚拟概念, 一个域是一组 User Group 或 Project 的容器                          |
|    User    | 用户, 是可以通过 Keystone 访问 OpenStack 服务的个人、系统或某个服务                             |
|   Group    | 用户组, 是一组 User 的容器, 可以向 Group 中添加用户, 并直接给 Group 分配角色                    |
|  Project   | 项目, 是各个服务中一些可以访问的资源集合, 项目只需在某个域下唯一即可                            |
|    Role    | 角色, 具有一组定义的用户权限和特权以执行一组特定操作, 角色不同, 被赋予的权限不同                |
|  Service   | 服务, 一种 OpenStack 服务, 服务会对外暴露一个或多个端点, 用户可以通过这些端点访问资源并执行操作 |
|  Endpoint  | 端点, 是指一个可以用来访问某个具体服务的网络地址                                                |
|   Token    | 令牌, 是允许访问特定资源的凭证                                                                  |
| Credential | 凭证, 确认用户身份的数据, 如用户的用户名和密码                                                  |

- 概念间逻辑关系

![](/HuaweiCloudCompute/img/Keystone-概念间逻辑关系.png)

- 作用

Keystone 在用户与 OpenStack 服务之间架起一座桥梁
用户从 Keystone 获取令牌及服务目录; 用户在访问服务时, 发送自己的令牌; 相关服务向 Keystone 求证令牌的合法性

- 交互关系

Keystone 为其他服务提供认证, 外部请求调用 OpenStack 内部的服务时, 需要先从 Keystone 获取到相应的 Token
OpenStack 内部不同项目间的调用也需要先从 Keystone 获取到认证后才能进行

- 架构

![](/HuaweiCloudCompute/img/Keystone-体系架构.png)

|       组件名        | 作用                                                     |
| :-----------------: | :------------------------------------------------------- |
|    Keystone API     | 接收外部请求                                             |
| Keystone Middleware | 缓存 Token 等, 减轻 Keystone Services 压力               |
|  Keystone Services  | 不同的 Service 提供不同的认证或鉴权服务                  |
|  Keystone Backends  | 实现 Keystone 服务, 不同的 Service 由不同的 Backend 提供 |
|  Keystone Plugins   | 提供密码、Token 等认证方式                               |

- 对象模型

![](/HuaweiCloudCompute/img/Keystone-对象模型.png)

对象模型作用

|  对象模型  | 作用                                                                                                                                                                                                                                                                                        |
| :--------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|  Service   | Keystone 是在一个或多个端点(Endpoint)上公开的一组内部服务(Service), 内部服务以组合方式使用, 除内部服务, Keystone 还负责与 OpenStack 其他服务进行交互, 提供一个或多个端点, 用户可以通过这些端点访问资源并执行操作                                                                            |
|  Identity  | 该服务提供身份凭据验证以及用户(User)和用户组(Group)的数据, User 是单个 OpenStack 服务使用者; 用户必须所属于某个特定域, 所有用户名不是 OpenStack 全局唯一, 仅是其所属域唯一; Group 把多个用户作为一个整体管理, 组本身必须所属于某个特定域, 所有组名不是 OpenStack 全局唯一, 仅是其所属域唯一 |
|  Resource  | 该服务提供有关项目(Project)和域(Domain)的数据; Project 是 OpenStack 资源拥有者的基本单元, OpenStack 所有资源都属于特定项目; Domain 把项目, 用户和组作为一个整体管理, 每种资源属于某个特定域, 默认域为 Default                                                                               |
| Assignment | 该服务提供有关角色(Role)和角色分配(Role Assignment)的数据; Role 规定最终用户可以获得的授权级别, 角色可以在域或项目级别授予, 可以在单个用户或组级别分配角色, 角色名称在域范围内唯一; Role Assignment 是一个三元组, 一个 Role, 一个 Resource 和一个 Identify                                  |
|   Token    | 该服务提供用户访问服务的凭证, 代表用户的账户信息; Token 一般包含 User 信息, Scope 信息(Project, Domain 和 Trust), Role 信息                                                                                                                                                                 |
|  Catalog   | 该服务提供用于查询端点(Endpoint)的端点注册表, 以便外部访问 OpenStack 服务; Endpoint 本质上是一个 URL, 提供服务入口, 分三种: Pulic 公共网络使用, internal 内网使用, admin 管理网络使用                                                                                                       |
|   Policy   | 每个 OpenStack 服务都在相关的策略文件中定义其资源的访问策略(Policy); 访问策略类似于 Linux 中的权限管理, 不同的角色的用户或用户组将会拥有不同的操作权限; 访问策略规则以 JSON 格式指定, 文件名为 policy.json, 存放在/etc/service_name/policy.json                                             |

对象模型分配关系示例
![](/HuaweiCloudCompute/img/Keystone-对象模型分配关系示例1.png)
![](/HuaweiCloudCompute/img/Keystone-对象模型分配关系示例2.png)

对象模型使用示例
![](/HuaweiCloudCompute/img/Keystone-对象模型使用示例.png)

- Keystone 认证

![](/HuaweiCloudCompute/img/Keystone-认证方式.png)

| 认证方式           | 解释                                                                                                                                                    |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 基于令牌的认证方式 | 最常用的 Keystone 认证方式, 使用方式简单; 在认证请求头中添加 x-Auth-Token 的 http 头, keystone 检查该请求头中的 token, 并与数据库中的令牌值进行对比验证 |
| 基于外部的认证方式 | 集成使用第三方认证系统, 在认证请求中添加"remote_user"信息                                                                                               |
| 基于本地的认证方式 | 默认认证方式, 即用户名和密码认证                                                                                                                        |

基于令牌的认证-UUID
![](/HuaweiCloudCompute/img/Keystone-基于令牌的认证-UUID.png)

基于令牌的认证-PKI
![](/HuaweiCloudCompute/img/Keystone-基于令牌的认证-PKI.png)

基于令牌的认证-PKIZ
![](/HuaweiCloudCompute/img/Keystone-基于令牌的认证-PKIZ.png)

基于令牌的认证-Fernet
![](/HuaweiCloudCompute/img/Keystone-基于令牌的认证-Fernet.png)

基于令牌认证方式对比

| Token 类型    | UUID   | PKI             | PKIZ            | Fernet   |
| :------------ | :----- | :-------------- | :-------------- | :------- |
| 大小          | 32byte | KB              | KB              | 255byte  |
| 本地认证      | 否     | 是              | 是              | 否       |
| keystone 负载 | 大     | 小              | 小              | 大       |
| 存储于数据库  | 是     | 是              | 是              | 否       |
| 携带信息      | 无     | user,catalog 等 | user,catalog 等 | user 等  |
| 加密方式      | 无     | 非对称加密      | 非对称加密      | 对称加密 |
| 是否压缩      | 否     | 否              | 是              | 否       |

- OpenStack 认证流程-创建 VM

![](/HuaweiCloudCompute/img/OpenStack-認證流程-創建VM.png)

基于角色的访问控制流程
![](/HuaweiCloudCompute/img/keystone-基于角色的访问控制流程.png)

基于角色的访问控制原理
![](/HuaweiCloudCompute/img/keystone-基于角色的访问控制原理.png)

Keystone 实现认证和权限控制
![](/HuaweiCloudCompute/img/keystone-认证和权限控制.png)

# 镜像服务 Glance

- 定位

依赖于 keystone 认证服务
提供发现, 注册和检索虚拟机镜像功能
提供的虚拟机实例镜像可以存放在不同地方, 如本地文件系统, swift 对象存储, cinder 块存储

- 作用

维护镜像信息(元数据和镜像本身)
对 VM 创建快照, 备份 VM 状态或者创建新镜像
查询, 获取镜像元数据和镜像本身
注册, 上传 VM 镜像(镜像创建, 上传, 下载和管理)
支持多种方式存储镜像

## 架构

架构
![](/HuaweiCloudCompute/img/Glance-架构.png)

架构简化
![](/HuaweiCloudCompute/img/Glance-架构简化.png)

作用

| 组件                       | 作用                                                                                                                          |
| :------------------------- | :---------------------------------------------------------------------------------------------------------------------------- |
| client                     | 使用 glance 服务器的任何应用程序, 接收请求并调用 glance-api                                                                   |
| rest api                   | 通过 rest 接口对外开放 glance 功能, 接收请求                                                                                  |
| glance domain controller   | 管理 glance 内部服务器, glance domain controller 分层实现特定任务, 如认证, 事件通知, 策略控制和数据库连接等                   |
| registry layer             | 实现 glance domain controller 于 DAL 之间的安全访问                                                                           |
| database abstraction layer | 提供 glance 与数据库之间的统一 API 接口                                                                                       |
| glance DB                  | glance DB 在所有组件之间共享, 存放管理, 配置信息等数据                                                                        |
| glance store               | 负责与外部存储后端或本地文件系统的交互, 持久化存储镜像文件, glance store 提供统一的接口来访问后端存储, 屏蔽不同后端存储的差异 |

## 工作原理和流程

镜像实例规格

| 名称           | 描述                                                                   |
| -------------- | ---------------------------------------------------------------------- |
| 镜像(image)    | 虚拟机镜像包含一个虚拟磁盘, 其上包含可引导的操作系统, 为虚拟机提供模板 |
| 实例(instance) | 实例是在 OpenStack 上运行的虚拟机                                      |
| 规格(flavor)   | 规格定义了实例的 CPU, RAM, 磁盘等资源                                  |

镜像磁盘格式
镜像添加到 glance 时, 必须指定虚拟机镜像的磁盘格式和容器格式

| 磁盘格式 | 描述                                                    |
| -------- | ------------------------------------------------------- |
| raw      | 一种非结构化的磁盘镜像格式                              |
| vhd      | VMware, Xen, Microsoft, VirtualBox 等使用的常见磁盘格式 |
| vhdx     | vhd 格式的增强版本, 支持更大的磁盘容量和其他功能        |
| vmdk     | 常见的磁盘格式                                          |
| vdi      | VirtualBox 和 QEMU 支持的磁盘格式                       |
| iso 光盘 | (如 CDROM)的存档格式                                    |
| ploog    | Virtuozzo 支持和使用的磁盘格式, 用于运行 OS Containers  |
| qcow2    | QEMU 支持的磁盘格式, 支持动态扩展和写时复制             |
| aki      | Amazon Kernel Image                                     |
| ari      | Amazon RamdiskImage                                     |
| ami      | AmazonMachineImage                                      |

容器格式

| 容器格式   | 描述                                                       |
| ---------- | ---------------------------------------------------------- |
| bare       | 这表明镜像中没有容器或元数据信封                           |
| ovf        | 这是 OVF 容器格式                                          |
| ova        | 这表明 Glance 中存储的是 OVA tar 归档文件                  |
| docker     | 这表明 Glance 中存储的是容器文件系统的 Docker tar 归档文件 |
| compressed | 未指定压缩文件的确切格式                                   |
| aki        | 这表明 Glance 中存储的是 Amazon 内核镜像                   |
| ari        | 这表明 Glance 中存储的是 Amazon 虚拟磁盘镜像               |
| ami        | 这表明 Glance 中存储的是 Amazon 机器镜像                   |

Glance 状态机(镜像状态机和任务状态机)

| 镜像状态       | 描述                                                                      |
| -------------- | ------------------------------------------------------------------------- |
| queued         | 已在 glance-registry 中保留镜像标识符, 但镜像数据未上传, 镜像大小未初始化 |
| saving         | 镜像的原始数据正在上传到 Glance 中                                        |
| uploading      | 对镜像调用了 import data-put 请求                                         |
| importing      | 导入镜像中, 但镜像尚未就绪                                                |
| active         | 镜像创建完成, 可以使用                                                    |
| deactivated    | 禁止任何非管理员用户访问镜像                                              |
| killed         | 镜像上传时出错, 镜像不可用                                                |
| deleted Glance | 保留了镜像信息, 但不能继续使用, 镜像在一定时间后会被自动清理掉            |
| pending_delete | 类似 deleted, Glance 尚未删除镜像数据, 处于该状态的镜像可恢复             |

| 任务状态   | 描述           |
| ---------- | -------------- |
| pending    | 任务挂起       |
| processing | 任务正在处理中 |
| success    | 任务执行成功   |
| failure    | 任务执行失败   |

Glance 状态机转化图

![](/HuaweiCloudCompute/img/Glance-状态机转化图.png)

镜像与实例交互流程

![](/HuaweiCloudCompute/img/Glance-镜像与实例交互流程.png)

## 镜像制作

- 手动制作镜像

| 步骤                                                     | 命令操作               |
| -------------------------------------------------------- | ---------------------- |
| 使用 virt-manager 创建一个 Ubuntu 18.04 虚拟机并安装系统 |
| 登录虚拟机并安装 cloud-init                              | apt install cloud-init |
| 虚拟机内部, 停止虚拟机                                   | shutdown -h now        |
| 预清理虚拟机                                             | virt-sysprep -d VM_ID  |
| 释放虚拟机定义                                           | virsh undefine VM_ID   |
| 制作镜像                                                 | qemu-img create        |
| 上传镜像                                                 | opemstack image create |

- 镜像制作工具

diskimage-builder : 自动化磁盘映像创建工具
packer : 适配多云平台的镜像
virt-builder : 快速创建新虚拟机的工具, 可以在几分钟或更短的时间内创建各种用于本地或云用途的虚拟
机镜像

- 镜像转换

镜像转换

```bash
qemu-img convert -f raw -O qcow2 image.img image.qcow2
```

| 镜像格式         | qemu-img 参数 |
| ---------------- | ------------- |
| QCOW2 (KVM,Xen)  | qcow2         |
| QED (KVM)        | qed           |
| RAW              | raw           |
| VDI (VirtualBox) | vdi           |
| VHD (Hyper-V)    | vpc           |
| VMDK (Vmware)    | vmdk          |

# 计算服务 Nova

- 定位

提供大规模, 可扩展, 按需自助服务的计算资源, 支持管理裸机, 虚拟机和容器
依赖于 keystone, Neutron, Glance 服务

Nova 即 openstack compute service, 负责提供计算资源的模块, 是 openstack 中的核心模块
Nova 不包括虚拟化软件, Nova 定义域地城虚拟化机制交互的驱动程序, 通过基于 Web 的 API 公开功能

- 作用

实施服务和相关库, 提供对计算资源(包括裸金属, 虚拟机和容器)的大规模可扩展, 按需自助服务
Nova 负责: 虚拟机生命周期管理; 其他计算资源生命周期管理
Nova 不负责: 城在虚拟机的物理主机自身的管理; 全面的系统状态监控

## Nova 架构

- 架构

![](/HuaweiCloudCompute/img/Nova-架构图.png)

- 物理部署实例

![](/HuaweiCloudCompute/img/Nova-物理部署实例.png)

无中心架构;
各组件无本地持久化状态;
可水平扩展;
通常 nova-api, nova-scheduler, nova-conductor 组件合并部署在控制节点;
通过增加控制节点和计算节点实现简单方便的系统扩容;

- 服务运行架构

![](/HuaweiCloudCompute/img/Nova-服务运行架构.png)

- 资源池管理架构

![](/HuaweiCloudCompute/img/Nova-资源池管理架构.png)

- API 组件

![](/HuaweiCloudCompute/img/Nova-组件API.png)

功能:
对外提供 restapi 接口, 接收和处理请求
对传入参数进行合法性校验和约束限制
对请求的资源进行配额的校验和预留
资源的创建, 更新, 删除查询等
虚拟机生命周期管理的入口

- Conductor 组件

![](/HuaweiCloudCompute/img/Nova-组件Conductor.png)

功能:
数据库操作, 解耦其他组件(Nova-Compute)数据库访问
Nova 复杂流程控制, 如创建, 冷迁移, 热迁移, 虚拟机规格调整, 虚拟机重建
其他组件的依赖, 如 nova-compute 需要 nova-conductor 启动成功后才能启动
其他组件的心跳定时写入

- Scheduler 组件

![](/HuaweiCloudCompute/img/Nova-组件Scheduler.png)

功能:
筛选和确定将虚拟机实例分配到哪台服务器
分配过程分两步: 过滤和权重, 通过过滤器选择满足条件的计算节点, 然后通过权重选择最优节点

- Compute 组件

![](/HuaweiCloudCompute/img/Nova-组件Compute.png)

功能:
Nova-Compute 框架: Manager, Driver
对接不同的虚拟化平台: KVM, VMware, Xen, LXC, QEMU 等等

## Nova 工作原理和流程

- 虚拟机状态类型

| 虚拟机状态类型 | 描述                                      |
| -------------- | ----------------------------------------- |
| vm_state       | 数据库中记录的虚拟机状态                  |
| task_state     | 当前虚拟机的任务状态, 一般为中间态或 None |
| power_state    | 从 hypervisor 获取的虚拟机真实状态        |
| status         | 对外呈现的虚拟机状态                      |

状态之间的关系

系统内部只记录 vm_state, task_state, power_state 三个状态
Status 是由 vm_state 和 task_state 联合生成

虚拟机状态组合

| vm_state | task_state                   | status      |
| -------- | ---------------------------- | ----------- |
| active   | rebooting                    | REBOOT      |
| active   | reboot_pending               | REBOOT      |
| active   | reboot_started               | REBOOT      |
| active   | rebooting_hard               | HARD_REBOOT |
| active   | reboot_pending_hard          | HARD_REBOOT |
| active   | reboot_started_hard          | HARD_REBOOT |
| active   | rebuild_block_device_mapping | REBUILD     |
| active   | rebuilding                   | REBUILD     |
| active   | rebuild_spawning             | REBUILD     |
| active   | migrating                    | MIGRATING   |
| active   | resize_prep                  | RESIZE      |
| active   | resize_migrating             | RESIZE      |
| active   | resize_migrated              | RESIZE      |
| active   | resize_finish                | RESIZE      |
| active   | default                      | ACTIVE      |
| stopped  | resize_prep                  | RESIZE      |
| stopped  | resize_migrating             | RESIZE      |
| stopped  | resize_migrated              | RESIZE      |
| stopped  | resize_finish                | RESIZE      |
| stopped  | default                      | SHUTOFF     |

虚拟机状态变迁图
![](/HuaweiCloudCompute/img/Nova-VM状态变迁图.png)

Nova 创建虚拟机流程
![](/HuaweiCloudCompute/img/Nova-创建VM流程.png)

Nova 调度过程
![](/HuaweiCloudCompute/img/Nova-调度过程.png)

Nova 过滤安排器
![](/HuaweiCloudCompute/img/Nova-过滤安排器.png)

Live Migration 原理
![](/HuaweiCloudCompute/img/Nova-LiveMigration原理.png)

## Nova 典型操作

| 分组               | 说明                                                                                                                                                                                                                   |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 虚拟机生命周期管理 | 虚拟机创建、删除、启动、关机、重启、重建、规格更改、暂停、解除暂停、挂起、继续、迁移、在线迁移、锁定、解锁、疏散、拯救、解拯救、搁置、删除搁置、恢复搁置、备份、虚拟机导出镜像、列表、详细信息、信息查询更改和密码修改 |
| 卷和快照管理操作   | 本质上是对 Cinder API 的封装. 卷创建、删除、列表、详细信息查询. 快照创建、删除、列表、详细信息查询                                                                                                                     |
| 虚拟机卷操作       | 虚拟机挂卷、虚拟机卸卷、虚拟机挂卷列表、虚拟机挂卷详细信息查询                                                                                                                                                         |
| 虚拟网络操作       | 本质上是对 Neutron API 的封装. 虚拟网络创建、删除、列表、详细信息查询                                                                                                                                                  |
| 虚拟机虚拟网卡操作 | 虚拟机挂载网卡、虚拟机卸载网卡、虚拟机网卡列表                                                                                                                                                                         |
| 虚拟机镜像的操作   | 本质上是对 Glance API 的封装, 支持镜像的创建、删除、列表、详细信息查询                                                                                                                                                 |
| 其他资源其他操作   | Flavor, 主机组, keypairs, quota 等                                                                                                                                                                                     |

Nova 操作对象

| 名称              | 简介                  | 说明                                                                                                                                                                                                                                   |
| ----------------- | --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Server            | 虚拟机                | Nova 管理提供的云服务资源, Nova 中最重要的数据对象                                                                                                                                                                                     |
| Server metadata   | 虚拟机元数据          | 通常用于为虚拟机附加必要描述信息, key/value 格式                                                                                                                                                                                       |
| Flavor            | 虚拟机规格模板        | 用于定义虚拟机类型, 如 2 个 vCPU、4 GB 内存、40 GB 本地存储空间的虚拟机. Flavor 由系统管理员创建, 供普通用户在创建虚拟机时使用                                                                                                         |
| Quota             | 资源配额              | 用于指定租户最多能够使用的逻辑资源上限                                                                                                                                                                                                 |
| Hypervisor / node | 节点                  | 对于 KVM、Xen 等虚拟化技术, 一个 node 即对应一个物理主机. 对于 vCenter, 一个 node 对应一个 cluster Host 主机对于 KVM、Xen 等虚拟化技术, 一个 host 即对应一个物理主机, 同时对应一个 node. 对于 vCenter, 一个 host 对应一套 vCenter 部署 |
| Host aggregate    | 主机组                | 一个 HA 内包含若干 host. 一个 HA 内的物理主机通常具有相同的 CPU 型号等物理资源特性                                                                                                                                                     |
| Server group      | 虚拟机亲和性/反亲和组 | 同一个亲和性组的虚拟机, 在创建时会被调度到相同的物理主机上. 同一个反亲和性组的虚拟机, 在创建时会被调度到不同的物理主机上                                                                                                               |
| Service           | Nova 各个服务         | 管理 nova 相关服务的状态, 包括 nova-compute、nova-conductor、nova-scheduler、nova-novncproxy、nova-consoleauth、nova-console                                                                                                           |
| BDM               | Block device mapping  | 块存储设备, 用于描述虚拟机拥有的存储设备信息                                                                                                                                                                                           |
| Image             | 镜像                  | 包含操作系统的文件, 用于创建虚拟机                                                                                                                                                                                                     |

# 存储服务 Cinder Swift

- OpenStack 存储类型

Ephemeral Storage 临时存储

1. 临时存储是指数据被虚拟机实例使用, 虚拟机实例被关机、重启或删除, 该实例中的所有数据信息会丢失
2. 如果只部署了 Nova 服务, 则默认分配给虚拟机的磁盘是临时的, 当虚拟机终止后, 存储空间也会被释放
3. 默认情况下, 临时存储以文件形式放置在计算节点的本地磁盘上

Persistent Storage 持久存储

1. 持久存储维护数据持续可用, 保证数据安全性, 持久化存储设备的生命周期独立于任何其他系统设备或资源, 无论虚拟机实例是否终止
2. 目前 OpenStack 的持久存储包括: 块存储、对象存储和文件系统存储

OpenStack 存储类型对比

| 用途         | 访问方式                     | 访问客户端                               | 管理服务   | 数据生命周期 | 存储设备容量 | 典型使用案例                                          |
| ------------ | ---------------------------- | ---------------------------------------- | ---------- | ------------ | ------------ | ----------------------------------------------------- | ------------------------------ |
| 临时存储     | 运行操作系统和提供启动空间   | 通过文件系统访问                         | 虚拟机     | Nova         | 虚拟机终止   | 管理员配置的 Flavor 指定容量                          | 如虚拟机第一块磁盘, 第二块磁盘 |
| 块存储       | 为虚拟机添加额外的持久化存储 | 块设备被分区, 格式化后挂载访问           | 虚拟机     | Cinder       | 被用户删除   | 用户创建时指定                                        | 1TB 磁盘                       |
| 对象存储     | 存储海量数据, 包括虚拟机镜像 | REST API                                 | 任何客户端 | Swift        | 被用户删除   | 可用物理存储空间和数据副本数据                        | 10TB 数据集存储                |
| 共享文件存储 | 为虚拟机添加额外的持久化存储 | 共享文件系统存储被分区, 格式化后挂载访问 | 虚拟机     | Manila       | 被用户删除   | 用户指定时创建;扩容时指定;用户配额指定;管理员指定容量 | NFS                            |

OpenStack 持久存储

| 类型             | 描述                                                                                                |
| ---------------- | --------------------------------------------------------------------------------------------------- |
| 块存储(Cinder)   | 操作对象是磁盘, 直接挂载到主机, 一般用于主机的直接存储空间和数据库应用, DAS 和 SAN 都可以提供块存储 |
| 对象存储(Swift)  | 操作内容是对象, 一个对象名称就是一个域名地址, 可以直接通过 restapi 的方式访问对象                   |
| 文件存储(Manila) | 操作对象是文件和文件夹, 在存储系统上增加了文件系统, 再通过 NFS 或 CIFS 协议访问                     |

- 作用

提供块存储服务, 为虚拟机实例提供持久化存储;
调用不同存储接口驱动, 将存储设备转化为块存储池, 用户无需了解存储的实际部署位置或设备类型;
Cinder 在虚拟机与具体存储设备之间引入一层"逻辑存储卷"的中间抽象层,为后端不同的存储技术提供统一的接口, Cinder 本身不是一种存储技术, 没有实现对块设备的实际管理和服务;
不同的块设备服务厂商在 Cinder 中以驱动的形式实现上述接口与 OpenStack 的整合

## 架构

- 架构图

![](/HuaweiCloudCompute/img/Cinder-架构图.png)

- 架构说明

![](/HuaweiCloudCompute/img/Cinder-架构说明.png)

- 架构部署(SAN 存储示例)

![](/HuaweiCloudCompute/img/Cinder-架构部署-SAN存储示例.png)

- API 组件

Cinder API 对外提供服务,对操作需求进行解析,并返回调用处理方法

| 类型 | 操作                        |
| ---- | --------------------------- |
| 卷   | create/delete/list/show     |
| 快照 | create/delete/list/show     |
| 卷   | attach/detach(Nova 调用)    |
| 其他 | Volume types/Quotas/Backups |

- Scheduler 组件
- Volume 组件

# 网络服务 Neutron

# 对象存储服务 Swift

# 编排服务 Heat

# 计量服务 Ceilometer
