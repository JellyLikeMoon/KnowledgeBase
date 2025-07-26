- [系统](#系统)
  - [sar](#sar)
  - [vmstat](#vmstat)
  - [sysctl](#sysctl)
  - [lsof](#lsof)
- [CPU](#cpu)
  - [mpstat](#mpstat)
- [内存](#内存)
  - [free](#free)
  - [slabtop](#slabtop)
- [磁盘](#磁盘)
  - [iostat](#iostat)
  - [dstat](#dstat)
  - [iotop](#iotop)
- [网络](#网络)
  - [netstat](#netstat)
  - [ss](#ss)
  - [tcpdump](#tcpdump)
- [进程](#进程)
  - [pmap](#pmap)
  - [pstree](#pstree)
  - [atop](#atop)
  - [htop](#htop)
  - [itrace](#itrace)
  - [top](#top)
  - [ps](#ps)
  - [pidstat](#pidstat)
  - [strace](#strace)

# 系统

## sar

_`sar` 是一个用于收集, 汇报和保存系统活动信息的工具. 它可以帮助管理员监控系统的性能, 包括 CPU 使用情况, 内存使用情况, I/O 操作, 网络流量等. sar 可以生成详细的报告, 帮助识别系统瓶颈并进行性能优化._

- 参数

```bash
-A                                      # 显示所有统计数据
-B                                      # 显示内存分页的统计数据 [A_PAGE]
-b                                      # 显示IO和传输速率的统计数据[A_IO]
-d                                      # 显示块设备的统计数据 [A_DISK]
-F [ MOUNT ]                            # 显示指定文件系统的统计数据 [A_FS]
-H                                      # 大内存页的统计数据 [A_HUGE]
-I { <int_list> | SUM | ALL }           # 中断统计数据 [A_IRQ]
-m { <keyword> [,...] | ALL }           # 电源管理统计数据 [A_PWR_...]
                                        # Keywords are:
                                        # CPU     CPU instantaneous clock frequency
                                        # FAN     Fans speed
                                        # FREQ    CPU average clock frequency
                                        # IN      Voltage inputs
                                        # TEMP    Devices temperature
                                        # USB     USB devices plugged into the system
-n { <keyword> [,...] | ALL }           # 网络统计数据 [A_NET_...]
                                        # Keywords are:
                                        # DEV     Network interfaces
                                        # EDEV    Network interfaces (errors)
                                        # NFS     NFS client
                                        # NFSD    NFS server
                                        # SOCK    Sockets (v4)
                                        # IP      IP traffic      (v4)
                                        # EIP     IP traffic      (v4) (errors)
                                        # ICMP    ICMP traffic    (v4)
                                        # EICMP   ICMP traffic    (v4) (errors)
                                        # TCP     TCP traffic     (v4)
                                        # ETCP    TCP traffic     (v4) (errors)
                                        # UDP     UDP traffic     (v4)
                                        # SOCK6   Sockets (v6)
                                        # IP6     IP traffic      (v6)
                                        # EIP6    IP traffic      (v6) (errors)
                                        # ICMP6   ICMP traffic    (v6)
                                        # EICMP6  ICMP traffic    (v6) (errors)
                                        # UDP6    UDP traffic     (v6)
                                        # FC      Fibre channel HBAs
                                        # SOFT    Software-based network processing
-q [ <keyword> [,...] | PSI | ALL ]     # 系统负载和系统压力统计数据
                                        # Keywords are:
                                        # LOAD    Queue length and load average statistics [A_QUEUE]
                                        # CPU     Pressure-stall CPU statistics [A_PSI_CPU]
                                        # IO      Pressure-stall I/O statistics [A_PSI_IO]
                                        # MEM     Pressure-stall memory statistics [A_PSI_MEM]
-r [ ALL ]                              # 内存利用率统计 [A_MEMORY]
-S                                      # 交换分区利用率统计 [A_MEMORY]
-u [ ALL ]                              # CPU利用率统计 [A_CPU]
-v                                      # 内核表统计 [A_KTABLES]
-W                                      # 内外存对换统计数据 [A_SWAP]
-w                                      # 任务创建和系统切换统计 [A_PCSW]
-y                                      # TTY设备统计 [A_SERIAL]
-P [cpu_list|ALL]                       # 显示指定PID和进程名的CPU利用率统计
-o                                      # 将统计信息写入文件
-f                                      # 从文件中读取统计信息
```

- 字段释义

```bash
CPU统计信息
%user                       # 用户态的 CPU 时间百分比
%nice                       # 以 nice 优先级执行的用户态 CPU 时间百分比
%system                     # 内核态的 CPU 时间百分比
%iowait                     # 等待 I/O 操作完成的时间百分比
%steal                      # 虚拟机窃取的 CPU 占用比
%idle                       # 空闲时间百分比

内存统计信息
kbmemfree                   # 空闲物理内存大小(KB)
kbmemused                   # 已使用物理内存大小(KB)
%memused                    # 已使用物理内存占总物理内存的百分比
kbbuffers                   # 缓冲区使用的内存大小(KB)
kbcached                    # 页面缓存使用的内存大小(KB)
kbcommit                    # 保证当前内存分配不会失败所需的内存总量(KB)
kbactive                    # 活跃内存大小(KB)
kbinact                     # 非活跃内存大小(KB)
kbdirty                     # 脏内存大小(KB)

交换分区统计信息
pswpin/s                    # 每秒从磁盘读入交换分区的数据量
pswpout/s                   # 每秒从交换分区写入磁盘的数据量

网络接口统计信息
IFACE                       # 网络接口名称
rxpck/s                     # 每秒接收的数据包数
txpck/s                     # 每秒发送的数据包数
rxkB/s                      # 每秒接收的数据量(KB)
txkB/s                      # 每秒发送的数据量(KB)
rxcmp/s                     # 每秒接收的压缩数据包数
txcmp/s                     # 每秒发送的压缩数据包数
rxmcst/s                    # 每秒接收的多播数据包数

系统调用和上下文切换统计信息
proc/s                      # 每秒创建的进程数
cswch/s                     # 每秒发生的上下文切换次数

系统负载和压力统计信息
runq-sz                     # 运行队列中的平均进程数
plist-sz                    # 进程列表中的平均进程数
ldavg-1                     # 过去 1 分钟的平均负载
ldavg-5                     # 过去 5 分钟的平均负载
ldavg-15                    # 过去 15 分钟的平均负载

```

- 使用示例

```bash
root@ubuntu:~# sar 1 1
Linux 5.15.0-119-generic (ubuntu)       08/23/2024      _x86_64_        (2 CPU)

11:15:05 AM     CPU     %user     %nice   %system   %iowait    %steal     %idle
11:15:06 AM     all      0.00      0.00      1.99      0.00      0.00     98.01
Average:        all      0.00      0.00      1.99      0.00      0.00     98.01

# 内存利用率统计
root@ubtest:~# sar -r 1 5
Linux 6.8.0-51-generic (ubtest)         01/18/2025      _x86_64_        (2 CPU)

03:48:45 PM kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:48:46 PM   2562328   3265488    348756      8.81     69224    811324    659752      8.33    360360    699604        32
03:48:47 PM   2562328   3265496    348748      8.81     69232    811324    660628      8.34    360972    699612         0
03:48:48 PM   2562328   3265496    348748      8.81     69232    811324    659356      8.32    360540    699612         0
03:48:49 PM   2562328   3265496    348748      8.81     69232    811324    660628      8.34    360528    699612         0
03:48:50 PM   2562328   3265496    348748      8.81     69232    811324    659356      8.32    360120    699612         0
Average:      2562328   3265494    348750      8.81     69230    811324    659944      8.33    360504    699610         6

# 交换分区利用率统计
root@ubtest:~# sar -S 1 5
Linux 6.8.0-51-generic (ubtest)         01/18/2025      _x86_64_        (2 CPU)

03:49:16 PM kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
03:49:17 PM   3959804         0      0.00         0      0.00
03:49:18 PM   3959804         0      0.00         0      0.00
03:49:19 PM   3959804         0      0.00         0      0.00
03:49:20 PM   3959804         0      0.00         0      0.00
03:49:21 PM   3959804         0      0.00         0      0.00
Average:      3959804         0      0.00         0      0.00

# 网络接口统计信息
root@ubtest:~# sar -n DEV 1 5
Linux 6.8.0-51-generic (ubtest)         01/18/2025      _x86_64_        (2 CPU)

03:49:47 PM     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s   %ifutil
03:49:48 PM        lo     20.00     20.00      1.91      1.91      0.00      0.00      0.00      0.00
03:49:48 PM     ens32     18.00     12.00      1.79      1.61      0.00      0.00      0.00      0.00

03:49:48 PM     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s   %ifutil
03:49:49 PM        lo      4.90      4.90      0.71      0.71      0.00      0.00      0.00      0.00
03:49:49 PM     ens32      5.88      2.94      0.77      0.52      0.00      0.00      0.00      0.00

03:49:49 PM     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s   %ifutil
03:49:50 PM        lo     11.00     11.00      1.04      1.04      0.00      0.00      0.00      0.00
03:49:50 PM     ens32      8.00      6.00      0.76      0.88      0.00      0.00      0.00      0.00

03:49:50 PM     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s   %ifutil
03:49:51 PM        lo     13.86     13.86      1.58      1.58      0.00      0.00      0.00      0.00
03:49:51 PM     ens32     13.86     10.89      1.84      1.36      0.00      0.00      0.00      0.00

03:49:51 PM     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s   %ifutil
03:49:52 PM        lo     19.80     19.80      1.99      1.99      0.00      0.00      0.00      0.00
03:49:52 PM     ens32     20.79     12.87      2.51      1.50      0.00      0.00      0.00      0.00

Average:        IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s   %ifutil
Average:           lo     13.89     13.89      1.44      1.44      0.00      0.00      0.00      0.00
Average:        ens32     13.29      8.93      1.53      1.17      0.00      0.00      0.00      0.00

# 统计信息写入文件
root@ubtest:~# sar -u -o cpu_usage.log 1 5
Linux 6.8.0-51-generic (ubtest)         01/18/2025      _x86_64_        (2 CPU)

03:50:39 PM     CPU     %user     %nice   %system   %iowait    %steal     %idle
03:50:40 PM     all      0.50      0.00      0.50      0.00      0.00     99.00
03:50:41 PM     all      0.50      0.00      0.50      0.00      0.00     99.00
03:50:42 PM     all      0.51      0.00      1.01      0.00      0.00     98.48
03:50:43 PM     all      0.00      0.00      1.03      0.00      0.00     98.97
03:50:44 PM     all      0.49      0.00      1.48      0.00      0.00     98.03
Average:        all      0.40      0.00      0.90      0.00      0.00     98.70

# 显示特定CPU的统计信息
root@ubtest:~# sar -P ALL 1 5
Linux 6.8.0-51-generic (ubtest)         01/18/2025      _x86_64_        (2 CPU)

03:51:39 PM     CPU     %user     %nice   %system   %iowait    %steal     %idle
03:51:40 PM     all      0.00      0.00      1.03      0.00      0.00     98.97
03:51:40 PM       0      0.00      0.00      1.03      0.00      0.00     98.97
03:51:40 PM       1      0.00      0.00      1.03      0.00      0.00     98.97

03:51:40 PM     CPU     %user     %nice   %system   %iowait    %steal     %idle
03:51:41 PM     all      0.50      0.00      0.50      0.00      0.00     99.00
03:51:41 PM       0      0.00      0.00      0.00      0.00      0.00    100.00
03:51:41 PM       1      0.99      0.00      0.99      0.00      0.00     98.02

03:51:41 PM     CPU     %user     %nice   %system   %iowait    %steal     %idle
03:51:42 PM     all      0.00      0.00      0.00      0.00      0.00    100.00
03:51:42 PM       0      0.00      0.00      0.00      0.00      0.00    100.00
03:51:42 PM       1      0.00      0.00      0.00      0.00      0.00    100.00

03:51:42 PM     CPU     %user     %nice   %system   %iowait    %steal     %idle
03:51:43 PM     all      0.00      0.00      1.00      0.00      0.00     99.00
03:51:43 PM       0      0.00      0.00      1.00      0.00      0.00     99.00
03:51:43 PM       1      0.00      0.00      1.00      0.00      0.00     99.00

03:51:43 PM     CPU     %user     %nice   %system   %iowait    %steal     %idle
03:51:44 PM     all      0.00      0.00      1.01      0.00      0.00     98.99
03:51:44 PM       0      0.00      0.00      1.00      0.00      0.00     99.00
03:51:44 PM       1      0.00      0.00      1.01      0.00      0.00     98.99

Average:        CPU     %user     %nice   %system   %iowait    %steal     %idle
Average:        all      0.10      0.00      0.70      0.00      0.00     99.19
Average:          0      0.00      0.00      0.60      0.00      0.00     99.40
Average:          1      0.20      0.00      0.80      0.00      0.00     98.99

# 显示系统调用和上下文切换统计信息
root@ubtest:~# sar -w 1 5
Linux 6.8.0-51-generic (ubtest)         01/18/2025      _x86_64_        (2 CPU)

03:53:16 PM    proc/s   cswch/s
03:53:17 PM      0.00    194.00
03:53:18 PM     10.89    215.84
03:53:19 PM      7.00    191.00
03:53:20 PM      0.00    130.69
03:53:21 PM      0.00    146.00
Average:         3.59    175.50

# 显示系统负载和压力统计信息
root@ubtest:~# sar -q 1 5
Linux 6.8.0-51-generic (ubtest)         01/18/2025      _x86_64_        (2 CPU)

03:56:24 PM   runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
03:56:25 PM         1       285      0.02      0.03      0.00         0
03:56:26 PM         0       287      0.02      0.03      0.00         0
03:56:27 PM         0       287      0.02      0.03      0.00         0
03:56:28 PM         0       284      0.02      0.03      0.00         0
03:56:29 PM         1       284      0.02      0.03      0.00         0
Average:            0       285      0.02      0.03      0.00         0
```

## vmstat

_`vmstat` 报告有关进程, 内存, 分页, 块 IO, 陷阱, 磁盘和 CPU 活动的信息._
_生成的第一份报告给出了自上次重启以来的平均值. 其他报告提供的信息取样周期长度为延迟. 无论是哪种情况, 进程和内存报告都是即时的._

- 参数

```bash
vmstat 1 1             # 1秒内进行1次采样
-a, --active           # 活跃/非活跃内存
-f, --forks            # 自启动到现在的进程创建次数(/proc/stat的processes字段)
-m, --slabs            # slab信息(/proc/slabinfo)
-n, --one-header       # 不重新显示标题
-s, --stats            # 内存相关事件计数器统计(/proc/meminfo,/proc/stat,/proc/vmstat)
-d, --disk             # 磁盘统计信息(/proc/diskstats)
-D, --disk-sum         # 汇总磁盘统计信息
-p, --partition <dev>  # 分区特定统计信息
-S, --unit <char>      # 定义显示单位:k K m M(Byte),默认为K(1024Bytes)
-w, --wide             # 宽格式输出
-t, --timestamp        # 显示时间戳
-y, --no-first         # 跳过输出的第一行
-h, --help             # 显示帮助信息
-V, --version          # 显示版本信息
```

- 字段释义

```bash
Procs:                  # 进程
r                       # 运行队列中进程数量(大于CPU核心数则会出现CPU瓶颈)
b                       # 等待IO的进程数量
Memory:                 # 内存(KB)
swpd                    # 使用虚拟内存大小
free                    # 可用物理内存大小
buff                    # 缓冲区的内存大小
cache                   # 作页面缓存的内存大小
Swap:                   # 交换分区(KB)
si                      # 每秒从磁盘写入内存的内存量
so                      # 每秒从内存写入磁盘的内存量
IO:                     # 输入输出(1KB磁盘块)
bi                      # 每秒读取到内存的块数
bo                      # 每秒写入到磁盘的块数
System:                 # 系统
in                      # 每秒中断的数量
cs                      # 每秒上下文切换数
Cpu:                    # CPU百分比
us                      # 用户进程执行时间
sy                      # 系统进程执行时间
id                      # 空闲时间
wa                      # IO等待时间
st                      # 虚拟机窃取的CPU占用比
```

- 使用示例

```bash
root@ubuntu:~#  vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 696572 144276 2601244    0    0     7    24   50   80  0  1 99  0  0

root@ubuntu:~# vmstat 5 5
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 711516 143284 2590324    0    0     7    23   49   79  0  1 99  0  0
 0  0      0 711264 143284 2590324    0    0     0     8  181  282  0  2 98  0  0
 0  0      0 711012 143284 2590324    0    0     0     0  170  255  0  2 98  0  0
 0  0      0 711012 143284 2590324    0    0     0     2  174  256  0  2 98  0  0
 0  0      0 711012 143284 2590324    0    0     0     2  166  251  0  2 98  0  0
```

## sysctl

`sysctl` 是一个用于配置和查看 Linux 内核运行时参数的工具. 它允许管理员动态调整内核参数, 而无需重新编译或重启系统.

> 作用:
> 配置内核参数: 通过修改`/proc/sys/`文件系统中的文件或使用 sysctl 命令来动态调整内核参数.
> 查看当前配置: 显示当前内核参数的值, 帮助管理员了解系统的当前配置状态.
> 持久化配置: 将配置保存到`/etc/sysctl.conf`或其他配置文件中, 确保系统重启后仍能生效.

- 配置文件

`/etc/sysctl.conf`: 主要的配置文件, 用于持久化内核参数设置.  
`/etc/sysctl.d/`: 目录下的配置文件, 可以按需创建多个配置文件, 方便管理和维护.  
`/usr/lib/sysctl.d/` 和 `/run/sysctl.d/`: 系统提供的配置文件, 默认优先级低于 `/etc/sysctl.d/`

- 参数

```bash
-a, --all                       # 显示所有变量
-A                              # "-a" 的别名
-X                              # "-a" 的别名
    --deprecated                # 包含弃用参数列表
    --dry-run                   # 打印键值但是不写入到配置文件
-b, --binary                    # 不换行打印值
-e, --ignore                    # 忽略未知变量错误
-N, --names                     # 仅打印变量名但不打印变量值
-n, --values                    # 仅打印指定变量名的值
-p, --load[=<file>]             # 从指定文件中读取值, 默认为"/etc/sysctl.conf"
-f                              # "-p" 的别名
    --system                    # 从所有系统目录中读取值
-r, --pattern <expression>      # 根据正则表达式匹配设置
-q, --quiet                     # 不回显变量设置
-w, --write                     # 临时将值写入到变量名
```

- 示例

```bash
# 查看所有当前有效的sysctl设置
sysctl -a

# 查看指定参数的值
sysctl net.ipv4.ip_forward

# 临时修改内核参数
sysctl -w net.ipv4.ip_forward=1

# 从指定配置文件加载并应用参数
sysctl -p /etc/sysctl.conf

# 从默认配置文件加载并应用参数
sysctl -p

# 将所有配置文件中的设置应用
sysctl --system

# 忽略无效的设置项而不报错
sysctl -e -p

# 仅显示值而不显示设置名称
sysctl -n net.ipv4.ip_forward

# 不换行打印值
sysctl -b net.ipv4.ip_forward

# 使用正则表达式过滤输出
sysctl -r '^net\.ipv4\.ip_forward'

# 减少冗余信息输出
sysctl -q -w net.ipv4.ip_forward=1
```

## lsof

_`lsof` 是 List Open File 的缩写, 它主要用来获取被系统进程打开文件的信息, lsof 命令可以查看所有已经打开了的文件, 比如: 普通文件, 目录, 特殊的块文件, 管道, socket 套接字, 设备, Unix 域套接字等等, 同时, 它还可以结合 grep 以及 ps 命令进行更多的高级搜索_

- 参数

```bash
lsof -u [username/UID]                                     # 显示指定用户或UID已经打开的文件
lsof -g GID                                                # 显示指定GID/组ID已打开的文件
lsof -u username | grep deleted                            # 显示已经删除的文件
lsof -i:port                                               # 显示指定端口号上打开的文件
lsof -i [46][proto][@host|addr][:svc_list|port_list]       # 显示指定网络情况打开的文件
lsof -p PID                                                # 显示指定进程PID打开的文件
lsof -r 5                                                  # 每5秒重新显示一次内容
lsof -c cmd                                                # 显示指定命令正在打开的文件和网络连接
lsof -d ID                                                 # 显示FD为ID的已打开文件
lsof /name                                                 # 显示打开文件name的进程,即查找某个文件相关的进程
lsof +D                                                    # 递归显示目录及文件的信息
lsof +d                                                    # 显示目录的信息
lsof -Z                                                    # 显示所有打开的文件并附加SELinux安全上下文
```

- 字段释义

```bash
COMMAND                       # 进程名
PID                           # 进程ID
TASKCMD                       # 线程名
TID                           # 线程ID
USER                          # 所属用户
FD                            # 文件描述符
                              # cwd: 当前工作目录；
                              # Lnn: 库引用 (AIX)；
                              # err: 文件描述符信息错误(参见NAME列)；
                              # jld: 监狱目录 (FreeBSD)；
                              # ltx: 共享库文本(代码和数据)；
                              # Mxx: 十六进制内存映射类型编号 xx.
                              # m86: DOS Merge 映射文件；
                              # mem: 内存映射文件；
                              # mma: 内存映射设备；
                              # pd : 父目录；
                              # rtd: 根目录；
                              # tr : 内核跟踪文件 (OpenBSD)；
                              # txt: 程序文本(代码和数据)；
                              # v86: VP/ix 映射文件；
                              # 0   : 标准输出
                              # 1   : 标准输入
                              # 2   : 标准错误
                              # u : 表示读取和写入模式
                              # r : 表示只读模式
                              # w : 表示写入模式
                              # - : 表示处于未知状态,且未被锁定
                              # N: 未知类型的 Solaris NFS 锁；
                              # r: 对文件部分进行读锁；
                              # R: 对整个文件进行读锁；
                              # w: 对文件部分进行写锁；
                              # W: 对整个文件进行写锁；
                              # u: 对任何长度的文件进行读写锁；
                              # U: 未知类型的锁；
                              # x: 对文件部分进行 SCO OpenServer Xenix 锁；
                              # X: 对整个文件进行 SCO OpenServer Xenix 锁；
TYPE                          # 文件类型
                              # DIR     : 目录
                              # REG     : 普通文件
                              # CHR     : 字符
                              # a_inode : inode文件
                              # FIFO    : 管道或socket文件
                              # netlink : 网络
                              # unkown  : 未知
DEVICE                        # 设备ID, 通常为主设备ID和次设备ID的组合
SIZE/OFF                      # 大小或偏移量
NODE                          # 文件inode号
NAME                          # 路径或链接
```

- 使用示例

```bash
root@debian:~# lsof -i:22
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    1351 root    3u  IPv4  30588      0t0  TCP *:ssh (LISTEN)
sshd    1351 root    4u  IPv6  30599      0t0  TCP *:ssh (LISTEN)
sshd    1370 root    4u  IPv4  31789      0t0  TCP 172.21.100.222:ssh->172.21.100.201:52836 (ESTABLISHED)
sshd    1372 root    4u  IPv4  31794      0t0  TCP 172.21.100.222:ssh->172.21.100.201:52837 (ESTABLISHED)

```

# CPU

## mpstat

_显示 CPU 的性能统计信息_

- 参数

```bash
-A                                  # 显示所有可用的统计信息
-P <number|ALL>                     # 显示指定CPU统计信息
-I <SUM|CPU|SCPU|ALL>               # 显示指定的统计信息类型(SUM:每个处理器的中断总数;CPU:每个核心的每秒中断数量;SCPU:每个核心每秒的软中断数量)
-T                                  # 显示拓扑元素
-o                                  # 显示统计信息为JSON格式
-u                                  # 显示CPU使用率
-n                                  # 根据NUMA节点显示CPU统计信息
```

- 字段释义

```bash
CPU                                 # 处理器编号, ALL为所有处理器之间计算的平均值
%user                               # 用户级别程序的CPU使用率百分比
%nice                               # nice值为负进程的CPU时间
%sys                                # 内核级别程序的CPU使用率百分比, 未包括硬件和软件中断服务所花费的时间
%iowait                             # CPU等待硬盘IO的时间
intr/s                              #
%irq                                # 硬中断时间
%soft                               # 软中断时间
%steal                              # 虚拟CPU非主动等待的时间
%guest                              # 虚拟CPU使用的时间
%gnice                              # 虚拟CPU运行一个nice的虚拟机使用的时间
%idle                               # CPU除去等待磁盘IO操作外的因为任何原因而空闲的时间闲置时间(%) (idle/total)*100
```

- 使用示例

```bash
root@ubuntu:~# mpstat
Linux 5.15.0-118-generic (ubuntu)       08/15/2024      _x86_64_        (2 CPU)

04:17:11 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
04:17:11 PM  all    0.05    0.00    1.14    0.02    0.00    0.25    0.00    0.00    0.00   98.54
```

# 内存

## free

_显示内存使用情况_

- 参数

```bash
-b, --bytes             # 以字节显示输出
    --kilo              # 以千字节显示输出
    --mega              # 以兆字节显示输出
    --giga              # 以吉字节显示输出
    --tera              # 以太字节显示输出
    --peta              # 以拍字节显示输出
-k, --kibi              # 以千字节（二进制）显示输出
-m, --mebi              # 以兆字节（二进制）显示输出
-g, --gibi              # 以吉字节（二进制）显示输出
    --tebi              # 以太字节（二进制）显示输出
    --pebi              # 以拍字节（二进制）显示输出
-h, --human             # 显示人类可读的输出
    --si                # 使用1000的幂而不是1024
-l, --lohi              # 显示详细的低和高内存统计信息
-L, --line              # 在单行上显示输出
-t, --total             # 显示RAM + 交换分区的总和
-v, --committed         # 显示已提交的内存和提交限制
-s N, --seconds N       # 每N秒重复打印一次
-c N, --count N         # 重复打印N次，然后退出
-w, --wide              # 宽输出
    --help              # 显示此帮助并退出
-V, --version           # 输出版本信息并退出

```

- 字段释义

```bash
Mem                  # 物理内存
Swap                 # 交换分区
total                # 总内存
used                 # 已使用内存
free                 # 未使用内存
shared               # 共享内存
buff/cache           # 缓冲与缓存
available            # 可用内存
```

- 使用示例

```bash
root@ubuntu:~# free
               total        used        free      shared  buff/cache   available
Mem:         3961020      728044     2269656        1480     1220876     3232976
Swap:        3984380           0     3984380

root@ubuntu:~# free --mega
               total        used        free      shared  buff/cache   available
Mem:            4056         746        2322           1        1250        3309
Swap:           4080           0        4080

root@ubuntu:~# free -L --mega
SwapUse           0 CachUse        1250  MemUse         747 MemFree        2321
```

## slabtop

_`slabtop` 是一个实时显示内核 slab 分配器状态的工具. 它可以帮助管理员监控和分析内存分配情况, 尤其是内核对象的缓存情况._

> 作用:
> 监控内存分配: 实时显示内核 slab 分配器的状态, 帮助管理员了解系统中各种内核对象的内存使用情况.
> 优化性能: 通过分析 slab 缓存的使用情况, 可以发现潜在的内存泄漏或不合理的内存分配, 从而进行优化.
> 调试问题: 当系统出现内存不足或性能瓶颈时, slabtop 可以帮助定位是由于 slab 分配器的问题导致的.

- 参数

```bash
-d SECONDS, --delay=N               # 设置刷新间隔(秒), 默认为3秒
-o, --once                          # 只刷新一次, 然后退出
-s SORT, --sort=S                   # 按指定字段排序, 可选值包括:
                                    # character   description         header
                                    # a           活动对象的数量       ACTIVE
                                    # b           每个slab的对象数     OBJ/SLAB
                                    # c           缓存大小             CACHE SIZE
                                    # l           slab的数量           SLABS
                                    # v           活动slab的数量       N/A
                                    # n           名称                 NAME
                                    # o           对象总数             OBJS
                                    # p           每个slab的页面数     N/A
                                    # s           对象大小             OBJ SIZE
                                    # u           缓存利用率           USE
```

- 字段释义

```bash
OBJS                # 当前分配的对象总数
ACTIVE              # 当前活跃的对象数
USE                 # 当前使用的对象数占活动对象数的百分比
OBJ SIZE            # 单个对象的大小(字节)
SLABS               # 当前分配的 slab 数量
OBJ/SLAB            # 每个 slab 中的对象数量
CACHE SIZE          # 缓存大小(字节)
NAME                # Slab组名称
```

- 使用示例

```bash
# 每5秒刷新一次
slabtop -d 5

# 按对象大小排序
slabtop -s s

# 按活动对象数量排序,并刷新5次退出
slabtop -s a -d 5

# 输出保存到文件
slabtop -d 5 > slabtop_output.txt

root@ubuntu:~# slabtop
 Active / Total Objects (% used)    : 1197718 / 1311062 (91.4%)
 Active / Total Slabs (% used)      : 29297 / 29297 (100.0%)
 Active / Total Caches (% used)     : 117 / 162 (72.2%)
 Active / Total Size (% used)       : 49382.53K / 53552.44K (92.2%)
 Minimum / Average / Maximum Object : 0.01K / 0.04K / 16.00K

  OBJS ACTIVE  USE OBJ SIZE  SLABS OBJ/SLAB CACHE SIZE NAME
 44508  44508 100%    0.19K   1724       26      6896K dentry
 43504  43504 100%    0.19K   1673       26      6692K inode_cache
 16384  16384 100%    0.09K    512       32      2048K kmalloc-96
 16384  16384 100%    0.09K    512       32      2048K kmalloc-192
 16384  16384 100%    0.09K    512       32      2048K kmalloc-256
 16384  16384 100%    0.09K    512       32      2048K kmalloc-512
 16384  16384 100%    0.09K    512       32      2048K kmalloc-1k
 16384  16384 100%    0.09K    512       32      2048K kmalloc-2k
 16384  16384 100%    0.09K    512       32      2048K kmalloc-4k
 16384  16384 100%    0.09K    512       32      2048K kmalloc-8k

```

# 磁盘

## iostat

_报告中央处理单元(CPU)统计信息以及设备和分区的输入/输出统计信息_

- 参数

```bash
-c                                        # 仅显示CPU利用情况
-d                                        # 仅显示磁盘利用情况
-h                                        # 使结果显示更人性化
-j <ID|LABEL|PATH|UUID> <device |ALL>     # 显示持久设备的名称
-k                                        # 以KB为单位显示
-m                                        # 以MB为单位显示
-N                                        # 显示磁盘阵列(LVM)信息
-o JSON                                   # 以JSON格式显示信息
-p <ALL|device>                           # 显示磁盘及分区信息
-s                                        # 显示简短的统计信息
-t                                        # 显示每份的报告时间
-x                                        # 显示详细的统计信息
```

- 字段释义

```bash
avg-cpu:
%user                               # 显示在用户级(应用程序)执行期间的 CPU 使用百分比.
%nice                               # 显示在用户级(应用程序)以 nice 优先级执行期间的 CPU 使用百分比.
%system                             # 显示在系统级(内核)执行期间的 CPU 使用百分比.
%iowait                             # 显示 CPU 或多个 CPU 在系统有未完成的磁盘 I/O 请求期间空闲的时间百分比.
%steal                              # 显示虚拟 CPU 或多个虚拟 CPU 在虚拟化管理程序(Hypervisor)服务其他虚拟处理器期间被迫等待的时间百分比.
%idle                               # 显示 CPU 或多个 CPU 在系统没有未完成的磁盘 I/O 请求期间空闲的时间百分比.
Device:

```

- 使用示例

```bash
root@ubuntu:~# iostat
Linux 5.15.0-118-generic (ubuntu)       08/16/2024      _x86_64_        (2 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.02    0.00    0.56    0.01    0.00   99.40

Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
dm-0              0.85        10.62        17.67         0.00     930039    1547600          0
loop0             0.00         0.00         0.00         0.00        362          0          0
loop1             0.00         0.02         0.00         0.00       2180          0          0
loop2             0.00         0.02         0.00         0.00       1611          0          0
loop3             0.00         0.00         0.00         0.00        364          0          0
loop4             0.00         0.01         0.00         0.00       1082          0          0
loop5             0.00         0.01         0.00         0.00       1122          0          0
loop6             0.00         0.00         0.00         0.00         10          0          0
sda               0.55        10.76        18.90         0.00     942466    1655880          0

```

## dstat

_`dstat` 是一个用于生成多种系统资源统计数据的工具, 它可以实时监控系统的 CPU, 磁盘, 网络, 内存等资源的使用情况. 它提供了比传统工具(如 vmstat, iostat 和 netstat)更丰富的信息, 并且可以同时显示多个资源的统计信息._

- 参数

```bash
-c, --cpu                               # 仅显示CPU统计信息
   -C 0,3,total                         # 包括cpu0、cpu3和总计
-d, --disk                              # 仅显示磁盘统计信息
   -D total,hda                         # 包括hda和总计
-g, --page                              # 仅显示页统计信息
-i, --int                               # 仅显示中断统计信息
   -I 5,eth2                            # 包括中断5和eth2使用的中断
-l, --load                              # 仅显示负载统计信息
-m, --mem                               # 仅显示内存统计信息
-n, --net                               # 仅显示网络统计信息
   -N eth1,total                        # 包括eth1和总计
-p, --proc                              # 仅显示进程统计信息
-r, --io                                # 仅显示IO统计信息 (已完成的I/O请求)
-s, --swap                              # 仅显示交换分区统计信息
   -S swap1,total                       # 包括swap1和总计
-t, --time                              # 仅显示当前系统日期及时间
-T, --epoch                             # 仅显示时间计数器 (自纪元以来的秒数)
-y, --sys                               # 仅显示系统统计信息
--aio                                   # 启用AIO统计信息
--fs, --filesystem                      # 启用文件系统统计信息
--ipc                                   # 启用IPC统计信息
--lock                                  # 启用锁统计信息
--raw                                   # 启用原始统计信息
--socket                                # 仅显示socket统计信息
--tcp                                   # 仅显示TCP统计信息
--udp                                   # 仅显示UDP统计信息
--unix                                  # 仅显示UNIX统计信息
--vm                                    # 启用虚拟内存统计信息
--vm-adv                                # 启用高级虚拟内存统计信息
--zones                                 # 启用区域信息统计信息
--list                                  # 列出所有可用插件
--<plugin-name>                         # 按名称启用外部插件（参见--list）
-a, --all                               # 等同于-cdngy（默认）
-f, --full                              # 自动扩展-C、-D、-I、-N和-S列表
-v, --vmstat                            # 等同于-pmgdsc -D total
--bits                                  # 强制使用位表示以字节表达的值
--float                                 # 强制屏幕显示浮点值
--integer                               # 强制屏幕显示整数值
--bw, --black-on-white                  # 更改为白色背景终端的颜色
--color                                 # 强制显示颜色
--nocolor                               # 关闭显示颜色
--noheaders                             # 禁用重复的标题
--noupdate                              # 禁用中间更新
--output file                           # 将显示内容输出为CSV格式文件
--profile                               # 在退出dstat时显示性能分析统计信息

```

- 字段释义

```bash
总CPU使用率:
usr                 # 用户态的CPU时间百分比
sys                 # 系统态的CPU时间百分比
idl                 # 空闲时间百分比
wai                 # 等待I/O的时间百分比
hi                  # 硬中断时间百分比
si                  # 软中断时间百分比
磁盘统计信息:
read                # 读取的数据量
write               # 写入的数据量
网络统计信息:
recv                # 接收的数据量
send                # 发送的数据量
分页统计信息:
in                  # 换入的页面数量
out                 # 换出的页面数量
系统统计信息:
int                 # 每秒的中断次数
csw                 # 每秒的上下文切换次数
```

- 示例

```bash
root@ubuntu:~# dstat
You did not select any stats, using -cdngy by default.
--total-cpu-usage-- -dsk/total- -net/total- ---paging-- ---system--
usr sys idl wai stl| read  writ| recv  send|  in   out | int   csw
  0   1  99   0   0| 135k  411k|   0     0 |   0     0 | 156   253
  1   2  97   0   0|   0     0 | 962B  974B|   0     0 | 202   314
  0   2  98   0   0|   0     0 | 860B  942B|   0     0 | 210   316
  1   2  97   0   0|   0     0 | 694B  692B|   0     0 | 187   268

root@ubtest:~# dstat -d
-dsk/total-
 read  writ
   0     0

root@ubtest:~# dstat -m
------memory-usage-----
 used  free  buf   cach
 451M 2488M   66M  791M

root@ubtest:~# dstat --list
timestamp plugins:
        epoch, epoch-adv, time, time-adv
/etc/pcp/dstat plugins:
        aio, battery, cpu, cpu-adv, cpu-use, disk, disk-avgqu, disk-avgrq, disk-svctm, disk-tps, disk-util, disk-wait, dm, dm-avgqu, dm-avgrq, dm-svctm,
        dm-tps, dm-util, dm-wait, entropy, freespace, fs, gpfs, gpfs-ops, innodb-buffer, innodb-io, innodb-ops, int, io, ipc, load, lock, md, md-avgqu,
        md-avgrq, md-svctm, md-tps, md-util, md-wait, mem, mem-adv, memcache, mongodb-conn, mongodb-dbstats, mongodb-latency, mongodb-mem,
        mongodb-opcount, mysql-io, mysql-keys, net, net-packets, nfs3, nfs3-ops, nfs4, nfs4-ops, nfsd3, nfsd3-ops, nfsd4, nfsd4-ops, page, part,
        part-avgqu, part-avgrq, part-svctm, part-tps, part-util, part-wait, postfix, proc, proc-count, raw, redis, redis-client, redis-mem, rpc, rpcd,
        socket, socket6, swap, sys, tcp, top-bio, top-bio-adv, top-childwait, top-cpu, top-cpu-adv, top-cputime, top-cputime-avg, top-io, top-io-adv,
        top-latency, top-latency-avg, top-mem, top-oom, udp, unix, utmp, vm, vm-adv, zfs-arc, zfs-l2arc, zfs-zil
```

## iotop

_`iotop` 是一个用于实时监控和显示 Linux 系统中 I/O 使用情况的工具. 它可以帮助管理员识别哪些进程或线程正在占用大量的磁盘 I/O 资源, 从而进行性能优化和故障排查. iotop 类似于 top 命令, 但专注于 I/O 操作, 而不是 CPU 和内存使用情况._

- 参数

```bash
-h, --help                         # 显示帮助信息
-o, --only                         # 仅显示正在执行的线程或进程,可按"o"键切换至显示全部进程或线程
-b, --batch                        # 非互动模式
-n NUM, --iter=NUM                 # 设置刷新频次,达到频次即退出
-d SEC, --delay=SEC                # 设置以秒为单位的刷新频次
-p PID, --pid=PID                  # 进程或线程列表监控情况,默认为全部
-u USER, --user=USER               # 用户列表的监控情况
-P, --processes                    # 仅显示进程,不显示线程
-a, --accumulated                  # 显示累计 I/O 而不是带宽. 在这种模式下, iotop 会显示自 iotop 启动以来进程的 I/O 数量.
-k, --kilobytes                    # 使用KB而非人为单位. 这种模式在编写 iotop 的批处理模式脚本时非常有用. iotop 不会选择最合适的单位, 而是以千字节为单位显示所有大小.
-t, --time                         # 每行添加时间戳
-q, --quiet                        # suppress some lines of header (implies --batch
--no-help                          # 显示帮助快捷键
```

- 字段释义

```bash
TID                     # 线程 ID 或进程 ID（如果使用 -P 参数）。
PRIO                    # 线程或进程的优先级。
USER                    # 线程或进程的所有者用户名。
DISK READ               # 每秒读取的数据量（KB/s）。
DISK WRITE              # 每秒写入的数据量（KB/s）。
SWAPIN                  # 从交换分区读取到物理内存的百分比。由于内核配置原因，可能无法显示此值。
IO>                     # 当前进程或线程是否正在进行 I/O 操作。如果是，则显示为 >。
COMMAND                 # 启动该进程或线程的命令名称。

```

- 使用示例

```bash
# 显示所有进程和线程的 I/O 使用情况
root@ubuntu:~# iotop
Total DISK READ:         0.00 B/s | Total DISK WRITE:         0.00 B/s
Current DISK READ:       0.00 B/s | Current DISK WRITE:      34.83 K/s
    TID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN     IO>    COMMAND
      1 be/4 root        0.00 B/s    0.00 B/s  ?unavailable?  init
      2 be/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [kthreadd]
      3 be/0 root        0.00 B/s    0.00 B/s  ?unavailable?  [rcu_gp]
      4 be/0 root        0.00 B/s    0.00 B/s  ?unavailable?  [rcu_par_gp]
      5 be/0 root        0.00 B/s    0.00 B/s  ?unavailable?  [slub_flushwq]
      6 be/0 root        0.00 B/s    0.00 B/s  ?unavailable?  [netns]
      8 be/0 root        0.00 B/s    0.00 B/s  ?unavailable?  [kworker/0:0H-events_highpri]
     10 be/0 root        0.00 B/s    0.00 B/s  ?unavailable?  [mm_percpu_wq]
     11 be/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [rcu_tasks_rude_]
     12 be/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [rcu_tasks_trace]
     13 be/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [ksoftirqd/0]
     14 be/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [rcu_sched]
     15 rt/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [migration/0]
     16 rt/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [idle_inject/0]
     18 be/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [cpuhp/0]
     19 be/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [cpuhp/1]
     20 rt/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [idle_inject/1]
     21 rt/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [migration/1]
     22 be/4 root        0.00 B/s    0.00 B/s  ?unavailable?  [ksoftirqd/1]
     24 be/0 root        0.00 B/s    0.00 B/s  ?unavailable?  [kworker/1:0H-events_highpri]
  keys:  any: refresh  q: quit  i: ionice  o: active  p: procs  a: accum
  sort:  r: asc  left: SWAPIN  right: COMMAND  home: TID  end: COMMAND
CONFIG_TASK_DELAY_ACCT not enabled in kernel, cannot determine SWAPIN and IO %

# 仅显示正在执行的线程和进程
root@ubtest:~# iotop -o

# 监控特定用户的所有进程
root@ubuntu:~# iotop -u username
Total DISK READ:         0.00 B/s | Total DISK WRITE:         0.00 B/s
Current DISK READ:       0.00 B/s | Current DISK WRITE:       0.00 B/s
    TID  PRIO  USER     DISK READ DISK WRITE>    COMMAND
      1 be/4 root        0.00 B/s    0.00 B/s init
      2 be/4 root        0.00 B/s    0.00 B/s [kthreadd]
      3 be/4 root        0.00 B/s    0.00 B/s [pool_workqueue_release]
      4 be/0 root        0.00 B/s    0.00 B/s [kworker/R-rcu_g]

# 监控特定进程
root@ubuntu:~# iotop -p 9
Total DISK READ:         0.00 B/s | Total DISK WRITE:         0.00 B/s
Current DISK READ:       0.00 B/s | Current DISK WRITE:       0.00 B/s
    TID  PRIO  USER     DISK READ DISK WRITE>    COMMAND
      9 be/0 root        0.00 B/s    0.00 B/s [kworker/0:0H-kblockd]

# 累计I/O模式, 显示自iotop启动以来每个进程的IO数量
root@ubuntu:~# iotop -a

# 添加时间戳
root@ubuntu:~# iotop -t

# 显示详细信息并保存到文件
root@ubuntu:~# iotop -ao > iotop_output.txt
```

# 网络

## netstat

_`netstat` 是 Linux 系统中用于显示网络连接, 路由表, 接口统计信息, 伪装连接和多播成员的命令行工具. 它提供了关于网络状态的详细信息, 帮助管理员监控和管理网络配置与性能._

- 参数

```bash
-r, --route                             # 显示路由表
-i, --interfaces                        # 显示接口表
-g, --groups                            # 显示多播组成员关系
-s, --statistics                        # 显示网络统计信息(如SNMP)
-M, --masquerade                        # 显示伪装的连接
-v, --verbose                           # 详细模式
-W, --wide                              # 不截断IP地址
-n, --numeric                           # 不解析名称
--numeric-hosts                         # 不解析主机名
--numeric-ports                         # 不解析端口名称
--numeric-users                         # 不解析用户名
-N, --symbolic                          # 解析硬件名称
-e, --extend                            # 显示其他/更多信息
-p, --programs                          # 显示套接字的PID/程序名
-o, --timers                            # 显示计时器
-c, --continuous                        # 持续列出
-l, --listening                         # 显示监听的服务器套接字
-a, --all                               # 显示所有套接字(默认: 已连接)
-F, --fib                               # 显示转发信息库(默认)
-C, --cache                             # 显示路由缓存而不是FIB
-Z, --context                           # 显示套接字的SELinux安全上下文
-t                                      # 显示UDP套接字
-u                                      # 显示UDP套接字
```

- 字段释义

```bash
Proto                       # 协议类型 (tcp, udp, raw, unix)
Recv-Q                      # 接收队列中的数据包数量
Send-Q                      # 发送队列中的数据包数量
Local Address               # 本地地址和端口
Foreign Address             # 远程地址和端口
State                       # 连接状态
                            # ESTABLISHED: 已建立连接
                            # LISTEN: 正在监听传入连接
                            # SYN_SENT: 已发送同步请求, 等待确认
                            # SYN_RECV: 已接收同步请求, 等待完成握手
                            # FIN_WAIT1: 已发送终止请求, 等待确认
                            # FIN_WAIT2: 已接收终止请求, 等待对方关闭
                            # TIME_WAIT: 等待足够的时间以确保对端收到 ACK
                            # CLOSE: 连接未打开
                            # CLOSE_WAIT: 已接收关闭请求, 等待应用程序关闭
                            # LAST_ACK: 已发送关闭请求, 等待确认
                            # CLOSING: 双方同时尝试关闭连接
                            # UNKNOWN: 未知状态
PID/Program name            # 使用该套接字的进程ID和程序名称
```

- 示例

```bash
# 显示所有TCP和UDP连接
root@ubuntu:~# netstat -atun
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0      0 172.20.100.129:22       172.20.100.1:56484      ESTABLISHED
tcp6       0      0 :::22                   :::*                    LISTEN
udp        0      0 127.0.0.53:53           0.0.0.0:*
udp        0      0 172.20.100.129:68       0.0.0.0:*
udp6       0      0 :::123                  :::*

# 显示所有侦听端口及其对应的进程
root@ubuntu:~# netstat -tulnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      17938/sshd
tcp6       0      0 :::22                   :::*                    LISTEN      17938/sshd
udp        0      0 127.0.0.53:53           0.0.0.0:*                           836/systemd-resolve
udp        0      0 172.20.100.129:68       0.0.0.0:*                           834/systemd-network
udp6       0      0 :::123                  :::*                                836/systemd-resolve

# 显示路由表信息
root@ubuntu:~# netstat -r
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
default         _gateway        0.0.0.0         UG        0 0          0 ens32
link-local      0.0.0.0         255.255.0.0     U         0 0          0 ens32
172.20.100.0    0.0.0.0         255.255.255.0   U         0 0          0 ens32

# 查看链接某服务端口最多的IP地址
netstat -ntu | grep :80 | awk '{print $5}' | cut -d: -f1 | awk '{++ip[$1]} END{for(i in ip) print ip[i],"\t",i}' | sort -nr

# tcp状态列表
netstat -nt | grep -e 127.0.0.1 -e 0.0.0.0 -e ::: -v | awk '/^tcp/ {++state[$NF]} END for(i in state) print i,"\t",state[i]}'
```

## ss

_`ss(Socket Statistics)`是 Linux 系统中用于转储套接字统计数据的命令. 它提供了比 netstat 更详细和更快速的网络连接信息, 特别适用于高负载环境下的性能监控和故障排查. ss 可以显示 TCP, UDP, UNIX 域套接字等不同类型套接字的状态, 并支持多种过滤和格式化选项._

- 参数

```bash
-h, --help          # 显示帮助信息
-V, --version       # 输出版本信息
-n, --numeric       # 不解析服务名称, 直接显示IP和Port
-r, --resolve       # 解析主机名
-a, --all           # 显示所有套接字
-l, --listening     # 显示监听中的套接字
-o, --options       # 显示定时器信息
-e, --extended      # 显示详细的套接字信息
-m, --memory        # 显示套接字内存使用情况
-p, --processes     # 显示使用套接字的进程
-T, --threads       # 显示使用套接字的线程
-i, --info          # 显示内部TCP信息
--tipcinfo          # 显示内部TIPC套接字信息
-s, --summary       # 显示套接字使用概要
--tos               # 显示TOS和优先级信息
--cgroup            # 显示cgroup信息
-b, --bpf           # 显示BPF过滤器套接字信息
-E, --events        # 持续显示被销毁的套接字
-Z, --context       # 显示任务的SELinux安全上下文
-z, --contexts      # 显示任务和套接字的SELinux安全上下文
-N, --net           # 切换到指定的网络命名空间

-4, --ipv4          # 仅显示IP版本4套接字
-6, --ipv6          # 仅显示IP版本6套接字
-0, --packet        # 仅显示PACKET套接字
-t, --tcp           # 仅显示TCP套接字
-M, --mptcp         # 仅显示MPTCP套接字
-S, --sctp          # 仅显示SCTP套接字
-u, --udp           # 仅显示UDP套接字
-d, --dccp          # 仅显示DCCP套接字
-w, --raw           # 仅显示RAW套接字
-x, --unix          # 仅显示Unix域套接字
--tipc              # 仅显示TIPC套接字
--vsock             # 仅显示vsock套接字
--xdp               # 仅显示XDP套接字
-f, --family=FAMILY # 显示指定类型的套接字
          FAMILY := { inet|inet6|link|unix|netlink|vsock|tipc|xdp|help }
```

- 字段释义

```bash
Netid               # 套接字类型(例如 tcp, udp, unix)
State               # 连接状态(例如 ESTAB, LISTEN, TIME-WAIT)
                    # ESTAB: 已建立连接
                    # SYN-SENT: 已发送同步请求, 等待确认
                    # SYN-RECV: 已接收同步请求, 等待完成握手
                    # FIN-WAIT-1: 已发送终止请求, 等待确认
                    # FIN-WAIT-2: 已接收终止请求, 等待对方关闭
                    # TIME-WAIT: 等待足够的时间以确保对端收到 ACK
                    # CLOSE: 连接未打开
                    # CLOSE-WAIT: 已接收关闭请求, 等待应用程序关闭
                    # LAST-ACK: 已发送关闭请求, 等待确认
                    # LISTEN: 正在监听传入连接
                    # CLOSING: 双方同时尝试关闭连接
                    # UNCONN: 未连接(通常用于 UDP)
Recv-Q              # 接收队列中的数据包数量
Send-Q              # 发送队列中的数据包数量
Local Address:Port  # 本地地址和端口
Peer Address:Port   # 对端地址和端口
Process             # 使用该套接字的进程信息(仅在使用 `-p` 参数时显示)
```

- 使用示例

```bash
# 显示UDP和TCP连接并显示进程信息, 并且不解析地址
root@ubuntu:~# ss -tupan
Netid     State      Recv-Q     Send-Q                  Local Address:Port              Peer Address:Port      Process
udp       UNCONN     0          0                       127.0.0.53%lo:53                     0.0.0.0:*          users:(("systemd-resolve",pid=836,fd=13))
udp       UNCONN     0          0                172.20.100.129%ens32:68                     0.0.0.0:*          users:(("systemd-network",pid=834,fd=17))
udp       ESTAB      0          0                      172.20.100.129:41901           172.20.100.254:53         users:(("systemd-resolve",pid=836,fd=11))
tcp       LISTEN     0          1024                        127.0.0.1:40257                  0.0.0.0:*          users:(("code-f1e16e1e62",pid=36777,fd=9))
tcp       LISTEN     0          128                           0.0.0.0:22                     0.0.0.0:*          users:(("sshd",pid=17938,fd=3))
tcp       LISTEN     0          4096                    127.0.0.53%lo:53                     0.0.0.0:*          users:(("systemd-resolve",pid=836,fd=14))
tcp       ESTAB      0          172                    172.20.100.129:22                172.20.100.1:56484      users:(("sshd",pid=36650,fd=4))
tcp       ESTAB      0          138                         127.0.0.1:40257                127.0.0.1:34452      users:(("code-f1e16e1e62",pid=36777,fd=12))
tcp       ESTAB      0          0                           127.0.0.1:34452                127.0.0.1:40257      users:(("sshd",pid=36650,fd=7))
tcp       LISTEN     0          128                              [::]:22                        [::]:*          users:(("sshd",pid=17938,fd=4))

# 显示UDP和TCP连接
root@ubuntu:~# ss -aut
Netid     State      Recv-Q     Send-Q                 Local Address:Port               Peer Address:Port       Process
udp       UNCONN     0          0                      127.0.0.53%lo:domain                  0.0.0.0:*
udp       UNCONN     0          0               172.20.100.129%ens32:bootpc                  0.0.0.0:*
udp       ESTAB      0          0                     172.20.100.129:41901            172.20.100.254:domain
tcp       LISTEN     0          1024                       127.0.0.1:40257                   0.0.0.0:*
tcp       LISTEN     0          128                          0.0.0.0:ssh                     0.0.0.0:*
tcp       LISTEN     0          4096                   127.0.0.53%lo:domain                  0.0.0.0:*
tcp       ESTAB      0          0                     172.20.100.129:ssh                172.20.100.1:56484
tcp       ESTAB      0          0                          127.0.0.1:40257                 127.0.0.1:34452
tcp       ESTAB      0          54                         127.0.0.1:34452                 127.0.0.1:40257
tcp       LISTEN     0          128                             [::]:ssh                        [::]:*

# 显示所有状态为established的ssh连接
root@ubtest:~# ss -o state established '( dport = :ssh or sport = :ssh )'
Netid      Recv-Q       Send-Q                       Local Address:Port                        Peer Address:Port       Process      
tcp        0            0                  [::ffff:172.21.100.110]:ssh              [::ffff:172.21.100.209]:53672       timer:(keepalive,107min,0)

# 显示进程使用的socket
root@ubtest:~# ss -pl
Netid State  Recv-Q Send-Q   Local Address:Port                  Peer Address:Port Process                                                                            
nl    UNCONN 0      0                 rtnl:kernel                          *                                                                                        
nl    UNCONN 0      0                 rtnl:systemd/1                       *                                                                                        
nl    UNCONN 0      0                 rtnl:systemd-resolve/649             *

# 找出打开套接字或端口的应用程序
root@ubtest:~# ss -lpn | grep ssh
u_str LISTEN 0      4096     /run/user/0/gnupg/S.gpg-agent.ssh 18804      * 0    users:(("systemd",pid=2400,fd=12))                              
tcp   LISTEN 0      4096                                     *:22         *:*    users:(("sshd",pid=2394,fd=3),("systemd",pid=1,fd=99))  
```

## tcpdump

_`tcpdump` 是一个强大的网络抓包和分析工具, 用于捕获和显示通过网络接口传输的数据包. 它可以帮助管理员监控网络流量, 排查网络问题以及进行安全审计. _

- 参数

```bash
-i interface                        # 指定监听的网络接口, 默认为第一个非环回接口
-n                                  # 不解析主机名, 直接显示IP地址
-nn                                 # 不解析主机名和服务端口号, 直接显示IP地址和端口号
-v                                  # 详细模式, 显示更详细的信息
-vv                                 # 更加详细的模式
-vvv                                # 最详细的模式
-c count                            # 捕获指定数量的数据包后退出
-s snaplen                          # 设置每个数据包的捕捉长度(默认65535字节)
-w file                             # 将捕获的数据包写入文件而不是立即显示
-r file                             # 从文件中读取数据包而不是实时捕获
-e                                  # 显示以太网帧头部信息
-q                                  # 简略模式, 只显示较少的信息
-t                                  # 不显示时间戳
-tt                                 # 显示未格式化的时间戳
-ttt                                # 显示相对时间戳(相对于上一个数据包)
-tttt                               # 显示完整的日期和时间戳
-X                                  # 显示数据包的十六进制和ASCII码表示
-XX                                 # 类似于 -X, 但还显示链路层头部
-A                                  # 显示数据包的ASCII码表示
-E spi@ipaddr algo:secret           # 解密IPsec ESP数据包
-p                                  # 不将接口设置为混杂模式
-B buffersize                       # 设置内核缓冲区大小(单位为KB)
-D                                  # 列出所有可用的网络接口
-F file                             # 使用指定文件中的BPF过滤表达式
-S                                  # 显示绝对TCP序列号
-T type                             # 强制将数据包解释为指定类型(如rpc, ftp等)
-V                                  # 显示版本信息并退出
-Z user                             # 以指定用户的身份运行
```

- 过滤表达式

`tcpdump` 支持使用 Berkeley Packet Filter (BPF) 表达式来过滤捕获的数据包. 常见的过滤表达式包括:

```bash
host <host>                          # 指定主机
net <net>[/<mask>]                   # 指定网络
port <port>                          # 指定端口
src host <host>                      # 指定源主机
dst host <host>                      # 指定目的主机
src port <port>                      # 指定源端口
dst port <port>                      # 指定目的端口
tcp                                  # 只捕获TCP协议的数据包
udp                                  # 只捕获UDP协议的数据包
icmp                                 # 只捕获ICMP协议的数据包
ether proto <proto>                  # 指定以太网协议
ip                                   # 只捕获IPv4协议的数据包
ip6                                  # 只捕获IPv6协议的数据包
not <expr>                           # 排除匹配的数据包
and, &&                              # 逻辑与
or, ||                               # 逻辑或
```

- 输出字段释义

```bash
timestamp                           # 数据包捕获的时间戳
source MAC address                  # 源MAC地址
destination MAC address             # 目的MAC地址
protocol                            # 协议类型(如TCP, UDP, ICMP等)
source IP address                   # 源IP地址
destination IP address              # 目的IP地址
source port                         # 源端口
destination port                    # 目的端口
sequence number                     # TCP序列号
acknowledgment number               # TCP确认号
flags                               # TCP标志位(如SYN, ACK, FIN等)
window size                         # TCP窗口大小
checksum                            # 校验和
payload data                        # 数据包的有效载荷
```

- 示例

```bash
# 捕获所有经过eth0接口的数据包
root@ubuntu:~# tcpdump -i eth0

# 捕获所有来自或发往192.168.1.1的数据包
root@ubuntu:~# tcpdump host 192.168.1.1

# 捕获所有来自或发往端口80的数据包
root@ubuntu:~# tcpdump port 80

# 捕获所有来自192.168.1.1且目的端口为80的数据包
root@ubuntu:~# tcpdump src host 192.168.1.1 and dst port 80

# 捕获所有TCP SYN数据包
root@ubuntu:~# tcpdump 'tcp[tcpflags] & (tcp-syn) != 0'

# 捕获所有ICMP数据包并将结果保存到文件
root@ubuntu:~# tcpdump icmp -w icmp_capture.pcap

# 从文件中读取数据包并显示
root@ubuntu:~# tcpdump -r icmp_capture.pcap

# 捕获前10个数据包后退出
root@ubuntu:~# tcpdump -c 10

# 捕获所有HTTP请求并显示详细信息
root@ubuntu:~# tcpdump -v -s 0 -A port 80

# 捕获所有DNS查询并显示详细信息
root@ubuntu:~# tcpdump -v -s 0 -A port 53

# 捕获所有SSH连接并显示详细信息
root@ubuntu:~# tcpdump -v -s 0 -A port 22

# 捕获所有ARP请求和响应
root@ubuntu:~# tcpdump arp

# 捕获所有TCP三次握手过程的数据包
root@ubuntu:~# tcpdump 'tcp[tcpflags] & (tcp-syn|tcp-ack) != 0'

# 捕获所有TCP FIN数据包
root@ubuntu:~# tcpdump 'tcp[tcpflags] & (tcp-fin) != 0'

# 捕获所有TCP RST数据包
root@ubuntu:~# tcpdump 'tcp[tcpflags] & (tcp-rst) != 0'
```

假设我们执行了以下命令来捕获数据包:

```bash
root@ubuntu:~# tcpdump -i eth0
```

输出示例:

```plaintext
15:47:15.123456 IP 192.168.1.100.54321 > 192.168.1.1.80: Flags [S], seq 123456789, win 64240, options [mss 1460,sackOK,TS val 1234567 ecr 0,nop,wscale 7], length 0
15:47:15.123457 IP 192.168.1.1.80 > 192.168.1.100.54321: Flags [S.], seq 987654321, ack 123456790, win 65535, options [mss 1460,sackOK,TS val 1234568 ecr 1234567,nop,wscale 7], length 0
15:47:15.123458 IP 192.168.1.100.54321 > 192.168.1.1.80: Flags [.], ack 987654322, win 64240, length 0
15:47:15.123459 IP 192.168.1.100.54321 > 192.168.1.1.80: Flags [P.], seq 123456790:123456890, ack 987654322, win 64240, length 100
15:47:15.123460 IP 192.168.1.1.80 > 192.168.1.100.54321: Flags [.], ack 123456890, win 65535, length 0
15:47:15.123461 IP 192.168.1.1.80 > 192.168.1.100.54321: Flags [P.], seq 987654322:987654422, ack 123456890, win 65535, length 100
```

- **时间戳**: 每行开头的时间戳表示数据包捕获的时间.
- **源 MAC 地址** 和 **目的 MAC 地址**: 通常在以太网帧头部显示, 除非使用 `-e` 选项.
- **协议**: 例如 `IP` 表示这是一个 IP 数据包.
- **源 IP 地址** 和 **目的 IP 地址**: 分别表示发送方和接收方的 IP 地址.
- **源端口** 和 **目的端口**: 分别表示发送方和接收方的端口号.
- **TCP 标志位**: 例如 `[S]` 表示 SYN 标志位, `[S.]` 表示 SYN-ACK 标志位, `[P.]` 表示带有数据的 ACK 标志位.
- **序列号** 和 **确认号**: 用于 TCP 连接的序列控制.
- **窗口大小**: 表示接收方可以接收的最大字节数.
- **选项**: 例如 MSS(最大段大小), SACK(选择性确认), TS(时间戳), NOP(无操作), WS(窗口缩放).
- **有效载荷长度**: 表示数据包中有效载荷的长度.

# 进程

## pmap

_`pmap` 是一个 Linux 系统下的命令行工具, 用于显示进程的内存映射关系. 它可以用来查看进程的内存使用情况, 包括映射的虚拟地址空间, 文件映射, 设备映射等._

- 参数

```bash
-x, --extended                  # 显示详细信息
-X                              # 显示更多详细信息
                                # 注意：格式根据 /proc/PID/smaps 变化
-XX                             # 显示内核提供的所有信息
-c, --read-rc                   # 读取默认的rc文件
-C, --read-rc-from=<file>       # 从指定文件读取rc文件
-n, --create-rc                 # 创建新的默认rc文件
-N, --create-rc-to=<file>       # 将新的rc文件创建到指定文件
                                # 注意：-n 和 -N 不允许带有pid参数
-d, --device                    # 显示设备格式
-q, --quiet                     # 不显示标题和页脚
-p, --show-path                 # 在映射中显示路径
-A, --range=<low>[,<high>]      # 将结果限制在给定范围内
-h, --help                      # 显示此帮助并退出
-V, --version                   # 输出版本信息并退出

```

- 字段释义

```bash
Address
Kbytes
RSS
Dirty
Mode
Mapping
```

- 使用示例

```bash
root@ubuntu:~# pmap -x 29506
29506:   sshd: root@notty
Address           Kbytes     RSS   Dirty Mode  Mapping
000064e9ed524000      48      48       0 r---- sshd
000064e9ed530000     580     580       0 r-x-- sshd
000064e9ed5c1000     252     248       0 r---- sshd
000064e9ed600000      16      16      16 r---- sshd
000064e9ed604000       4       4       4 rw--- sshd
000064e9ed605000       8       8       8 rw---   [ anon ]
000064ea1e6c4000     392     392     392 rw---   [ anon ]
000064ea1e726000     508     476     476 rw---   [ anon ]

```

## pstree

_`pstree` 是一个 Linux 系统下的命令行工具, 用于显示进程的树状结构. 它可以用来查看进程之间的关系, 包括父子关系, 兄弟关系等._

- 参数

```bash
-a, --arguments                 # 显示命令行参数
-A, --ascii                     # 使用ASCII字符绘制线条
-c, --compact-not               # 不压缩相同的子树
-C, --color=TYPE                # 按属性为进程着色
                                # （例如：进程年龄）
-g, --show-pgids                # 显示进程组ID；隐含-c选项
-G, --vt100                     # 使用VT100字符绘制线条
-h, --highlight-all             # 高亮显示当前进程及其祖先
-H PID, --highlight-pid=PID     # 高亮显示此进程及其祖先
-l, --long                      # 不截断长行
-n, --numeric-sort              # 按PID排序输出
-N TYPE, --ns-sort=TYPE         # 按指定命名空间类型排序输出
                                # （cgroup, ipc, mnt, net, pid, time, user, uts）
-p, --show-pids                 # 显示PID；隐含-c选项
-s, --show-parents              # 显示选定进程的父进程
-S, --ns-changes                # 显示命名空间转换
-t, --thread-names              # 显示完整的线程名称
-T, --hide-threads              # 隐藏线程，仅显示进程
-u, --uid-changes               # 显示UID转换
-U, --unicode                   # 使用UTF-8（Unicode）字符绘制线条
-V, --version                   # 显示版本信息
-Z, --security-context          # 显示安全属性
PID                             # 从这个PID开始；默认是1（init）
USER                            # 仅显示属于该用户的进程树
```

- 字段释义

```bash

```

- 使用示例

```bash
root@ubuntu:~# pstree
systemd─┬─VGAuthService
        ├─agetty
        ├─cron
        ├─dbus-daemon
        ├─multipathd───6*[{multipathd}]
        ├─polkitd───3*[{polkitd}]
        ├─rsyslogd───3*[{rsyslogd}]
        ├─sh───node─┬─node─┬─bash───pstree
        │           │      ├─sh───cpuUsage.sh───sleep
        │           │      └─11*[{node}]
        │           ├─node─┬─node───6*[{node}]
        │           │      └─11*[{node}]
        │           └─10*[{node}]
        ├─snapd───9*[{snapd}]
        ├─sshd───sshd───sh─┬─code-cd4ee3b1c3───2*[{code-cd4ee3b1c3}]
        │                  └─sleep
        ├─systemd───(sd-pam)
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-network
        ├─systemd-resolve
        ├─systemd-timesyn───{systemd-timesyn}
        ├─systemd-udevd
        ├─unattended-upgr───{unattended-upgr}
        └─vmtoolsd───3*[{vmtoolsd}]

```

## atop

## htop

## itrace

## top

_`top` 是一个实时显示 Linux 系统中各个进程资源使用情况的命令行工具. 它可以帮助系统管理员监控系统的性能, 包括 CPU, 内存和进程状态等信息. 通过 top, 用户可以查看当前系统中哪些进程占用了最多的资源, 并根据需要进行管理和优化._

- 参数

```bash
-d 1                          # 表示显示页面刷新一次的间隔为1秒
-n                            # 与"-b"配合使用,表示进行几次输出结果
-p                            # 指定"PID"进程号进行显示
-s                            # 安全模式运行,无法交互
-i                            # 不显示任何闲置或僵尸进程
-c                            # 显示完成命令行而非命令名称
-S                            # 指定累计模式
```

- top 操作参数

```bash
?/h                           # 显示当前可输入的命令
P                             # 以CPU使用率进行排序显示
M                             # 以内存使用率进行排序显示
N                             # 以PID进行排序显示
T                             # 以进程使用的时间累计排序显示
r                             # 给选定PID指定"nice值"(优先级)
K                             # 输入需要终止的进程PID终止进程
i                             # 忽略闲置或僵尸进程
c                             # 切换显示命令名称和完成命令行
t                             # 切换显示进程和CPU状态信息
m                             # 切换显示内存信息
l                             # 切换显示平均负载和启动时间信息
o或O                          # 改变显示项目的顺序
q                             # 退出

```

- 字段释义

```bash
top
00:00:00                      # 当前时间,
up                            # 系统运行时间
user                          # 当前登录用户数
load average                  # 系统平均负载: 1分钟 5分钟 15分钟
Tasks
total                         # 进程总数
running                       # 正在运行进程数
sleeping                      # 睡眠状态进程数
stopped                       # 终止状态进程数
zombie                        # 僵尸状态进程数
Cpu
us                            # 运行非nice的用户进程CPU时间占比
sy                            # 内核进程CPU时间占比
ni                            # 改变过优先级的用户进程CPU时间占比
id                            # 空闲CPU时间占比
wa                            # 等待IO的进程CPU时间占比
hi                            # 硬中断服务的CPU时间占比
si                            # 软中断服务的CPU时间占比
st                            # 被hypervisor占用的CPU时间占比
Mem
total                         # 总物理内存
free                          # 空闲内存量
used                          # 已使用物理内存量
buff/cache                    # 用于缓冲和缓存的内存量
Swap
total                         # 交换分区总量
free                          # 空闲交换分区量
used                          # 已使用交换分区量
avail Mem                     # 缓冲的交换分区量
输出字段
PID                           # 进程标识符
USER                          # 进程所属用户
PR                            # 进程优先级
NI                            # 进程 nice 值(正数的nice值表示高优先级,负数则反之,零表示无调整)
VIRT                          # 进程虚拟内存量(包括代码,数据,共享库,交换出的分页)
RES                           # 进程使用的非交换的物理内存(RES=CODE+DATA)
SHR                           # 进程使用的共享内存量(只反映可以和其他进程共享的内存)
S                             # 进程状态: D 不可中断的睡眠;R 正在运行;S 睡眠;T 跟踪/停止;Z 僵尸
%CPU                          # 进程的CPU使用率
%MEM                          # 进程的内存使用率
TIME+                         # 自系统启动开始任务所使用的总CPU时间,以1/100s显示
COMMAND                       # 启动进程的命令行或相关程序的名称
```

> 注意:
>
> - Load Average 是指单位时间内等待 CPU 资源的进程数的平均值. 它不仅包括正在使用 CPU 的进程, 还包括那些处于不可中断状态(如等待 I/O 操作完成)的进程.
> - Load average 阈值取决于 CPU 核心数. 例如, 对于 4 核心的系统:
>   - load average 为 2, 说明平均有两个进程在竞争 CPU 资源, 但仍有两核是空闲的
>   - load average 为 4, 则意味着每个核心都处于满负荷状态
>   - load average 超过 4, 说明系统已经超载, 可能会出现明显的性能瓶颈;

- 使用示例

```bash
top - 15:47:15 up 52 min,  1 user,  load average: 0.04, 0.09, 0.08
Tasks: 223 total,   1 running, 222 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  3.5 sy,  0.0 ni, 96.4 id,  0.0 wa,  0.0 hi,  0.2 si,  0.0 st
MiB Mem :   3876.4 total,   1900.5 free,    499.2 used,   1476.7 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.   3129.1 avail Mem

PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
3078 root      20   0 1360488 103464  46340 S   3.7   2.6   0:17.56 node
4076 root      20   0   32124  16464  11936 S   2.7   0.4   0:05.93 code-f1e16e1e62
4100 root      20   0 1332108  89104  42136 S   1.7   2.2   0:13.47 node
3117 root      20   0 1399520  62232  41396 S   1.3   1.6   0:03.15 node
4111 root      20   0 1240612  51984  40516 S   1.0   1.3   0:03.34 node
2922 root      20   0       0      0      0 I   0.7   0.0   0:00.40 kworker/1:1-events
2939 root      20   0       0      0      0 I   0.7   0.0   0:01.56 kworker/0:1-mm_percpu_wq
 14  root      20   0       0      0      0 I   0.3   0.0   0:16.33 rcu_sched
4002 root      20   0   17364  10956   8476 S   0.3   0.3   0:02.39 sshd
4210 root      20   0       0      0      0 I   0.3   0.0   0:00.72 kworker/u256:0-events_power_efficient
4266 root      20   0   10612   3772   3168 R   0.3   0.1   0:00.03 top
  1  root      20   0  102032  13120   8376 S   0.0   0.3   0:05.19 systemd
  2  root      20   0       0      0      0 S   0.0   0.0   0:00.02 kthreadd
  3  root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp
```

## ps

_`ps(Process Status)`是 Linux 系统中用于显示当前系统进程状态的命令. 它提供了有关正在运行的进程的详细信息, 包括进程 ID, 用户, CPU 和内存使用情况等. 通过 ps 命令, 用户可以查看哪些进程正在运行, 它们的状态以及资源使用情况, 从而进行进程管理和故障排查._

- 参数

```bash
-A, -e               # 所有进程
-a                   # 所有tty的进程,但不包括root
 a                   # 所有tty的进程包括其他用户
-d                   # 除了root的进程
-N, --deselect       # 排除选择的进程
 r                   # 仅正在运行的进程
 T                   # 当前终端正在运行的进程
 x                   # 显示没有控制终端的进程
-l                   # 长格式
-f                   # 全格式,包括命令行
-C                   # 根据命令行进行筛选
-p                   # 根据PID进行筛选

```

- 字段释义

```bash
USER         # 进程所有者的用户名
PID          # 进程 ID (Process ID)
%CPU         # 进程占用的 CPU 百分比
%MEM         # 进程占用的物理内存百分比
VSZ          # 进程使用的虚拟内存大小(KB)
RSS          # 进程占用的物理内存大小(KB)
TTY          # 终端名称(如果进程与终端关联, ? 表示无关联终端)
STAT         # 进程状态
             # R: 运行或可运行(在运行队列中)
             # S: 睡眠(等待某个事件发生)
             # D: 不可中断的睡眠(通常等待 I/O 操作完成)
             # I: 空闲内核进程
             # T: 停止(被信号暂停)
             # Z: 僵尸进程(已经终止但父进程尚未回收其状态)
             # <: 高优先级进程
             # N: 低优先级进程
             # L: 有页面锁定的进程
             # s: 具有子进程
             # l: 多线程进程
             # +: 前台进程组中的进程
START        # 进程启动时间
TIME         # 进程累计占用的 CPU 时间
COMMAND      # 启动该进程的命令行
```

- 示例

```bash
root@debian:~# ps -aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.3 101880 12076 ?        Ss   22:02   0:00 /sbin/init
root           2  0.0  0.0      0     0 ?        S    22:02   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   22:02   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   22:02   0:00 [rcu_par_gp]
root           5  0.0  0.0      0     0 ?        I<   22:02   0:00 [slub_flushwq]
root           6  0.0  0.0      0     0 ?        I<   22:02   0:00 [netns]
root           8  0.0  0.0      0     0 ?        I<   22:02   0:00 [kworker/0:0H-events_highpri]
root          10  0.0  0.0      0     0 ?        I<   22:02   0:00 [mm_percpu_wq]
root          11  0.0  0.0      0     0 ?        I    22:02   0:00 [rcu_tasks_kthread]
root          12  0.0  0.0      0     0 ?        I    22:02   0:00 [rcu_tasks_rude_kthread]
root          13  0.0  0.0      0     0 ?        I    22:02   0:00 [rcu_tasks_trace_kthread]

root@debian:~# ps -efl
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 22:02 ?        00:00:00 /sbin/init
root           2       0  0 22:02 ?        00:00:00 [kthreadd]
root           3       2  0 22:02 ?        00:00:00 [rcu_gp]
root           4       2  0 22:02 ?        00:00:00 [rcu_par_gp]
root           5       2  0 22:02 ?        00:00:00 [slub_flushwq]
root           6       2  0 22:02 ?        00:00:00 [netns]
root           8       2  0 22:02 ?        00:00:00 [kworker/0:0H-events_highpri]
root          10       2  0 22:02 ?        00:00:00 [mm_percpu_wq]
root          11       2  0 22:02 ?        00:00:00 [rcu_tasks_kthread]
root          12       2  0 22:02 ?        00:00:00 [rcu_tasks_rude_kthread]
root          13       2  0 22:02 ?        00:00:00 [rcu_tasks_trace_kthread]

root@debian:~# ps -A
PID TTY          TIME CMD
  1 ?        00:00:00 systemd
  2 ?        00:00:00 kthreadd
  3 ?        00:00:00 rcu_gp
  4 ?        00:00:00 rcu_par_gp
  5 ?        00:00:00 slub_flushwq
  6 ?        00:00:00 netns
  8 ?        00:00:00 kworker/0:0H-events_highpri
```

## pidstat

_`pidstat`是 Linux 系统中用于显示 Linux 进程的统计信息的命令. 它提供了有关正在运行的进程的统计信息, 包括 CPU 利用情况, 内存使用情况, 磁盘 IO 使用情况等. 通过 pidstat 命令, 用户可以查看哪些进程正在运行, 它们的状态以及资源使用情况, 从而进行进程管理和故障排查._

- 参数

```bash
pidstat 1 1                        # 1秒内采集1次数据
-u                                 # 显示CPU利用情况
-d                                 # 显示磁盘IO使用情况
-r                                 # 显示页面错误和内存利用情况
-R                                 # 显示实时优先级和调度策略信息
-s                                 # 显示堆栈利用情况
-t                                 # 显示指定任务相关的线程统计信息
-v                                 # 显示某些内核表的数值
-w                                 # 显示进程上下文切换活动
-C <command>                       # 指定命令行进行筛选
-G <process_name>                  # 指定进程名称进行筛选
-U <username>                      # 指定用户名进行筛选
-p <pid|ALL|SELF>                  # 指定进程PID进行筛选
-l                                 # 显示进程命令行和所有参数
--human                            # 显示方式增加百分号
-H                                 # 显示时间戳
```

- 字段释义

```bash
UID                                # 用户UID
USER                               # 用户名称
PID                                # 进程号
%usr                               # 该进程的用户空间占用CPU百分比
%system                            # 该进程的系统空间占用CPU百分比
%guest                             # 虚拟机占用CPU百分比
%wait                              # 任务等待运行时占用CPU百分比
%CPU                               # 进程使用的总CPU百分比
CPU                                # 处理该进程的处理器编号
COMMAND                            # 当前进程使用的命令名称
```

- 使用示例

```bash
root@ubuntu:~# pidstat
Linux 5.15.0-118-generic (ubuntu)       08/15/2024      _x86_64_        (2 CPU)

11:36:52 AM   UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
11:36:52 AM     0         1    0.04    0.22    0.00    0.03    0.27     0  systemd
11:36:52 AM     0         2    0.00    0.00    0.00    0.00    0.00     0  kthreadd
11:36:52 AM     0        13    0.00    0.02    0.00    0.04    0.02     0  ksoftirqd/0
11:36:52 AM     0        14    0.00    1.08    0.00    0.33    1.08     0  rcu_sched
11:36:52 AM     0        15    0.00    0.00    0.00    0.01    0.00     0  migration/0
11:36:52 AM     0        21    0.00    0.00    0.00    0.01    0.00     1  migration/1
11:36:52 AM     0        22    0.00    0.04    0.00    0.02    0.04     1  ksoftirqd/1
11:36:52 AM     0        32    0.00    0.08    0.00    0.04    0.08     0  kcompactd0
11:36:52 AM     0        38    0.00    0.01    0.00    0.06    0.01     1  kworker/1:1-events
11:36:52 AM     0        91    0.00    0.01    0.00    0.00    0.01     1  kworker/1:1H-kblockd
```

## strace

_`strace`是一个用于诊断, 调试和教学的实用工具. 它跟踪系统调用和信号, 可以用来查看程序执行过程中与操作系统内核的交互情况._

> 作用:
> 调试程序: 通过跟踪系统调用, 可以帮助开发者了解程序的行为, 尤其是当程序出现问题时.
> 性能分析: 可以查看程序在执行过程中花费时间最多的系统调用, 从而进行优化.
> 学习工具: 对于学习 Linux 系统调用和库函数的人来说, strace 是一个非常有用的工具.

- 参数

```bash
-c              # 统计每个系统调用的时间, 调用次数和错误次数, 并在程序结束时显示汇总统计信息
-e expr         # 指定要跟踪的系统调用或信号表达式
-f              # 跟踪由fork, vfork或clone创建的子进程
-F              # 跟踪由fork, vfork或clone创建的子进程, 并显示进程ID
-o filename     # 将输出重定向到指定文件
-p pid          # 附加到指定的进程ID并开始跟踪
-s bytes        # 设置输出字符串的最大长度(默认为32)
-t              # 在每一行前面加上时间戳
-T              # 显示每个系统调用所花费的时间
-v              # 详细模式, 显示系统调用的结构体和指针内容
-x              # 以十六进制格式显示数据
```

- 示例

```bash
# 跟踪所有系统调用
strace ls

# 跟踪特定的系统调用
strace -e trace=open,read,write ls

# 跟踪子进程
strace -f ./my_program

# 附加到现有进程
strace -p $(pidof my_program)

# 将输出保存到文件
strace -o strace_output.txt ls

# 统计系统调用
strace -c ls

# 显示时间戳
strace -t ls

# 显示系统调用耗时
strace -T ls

```
