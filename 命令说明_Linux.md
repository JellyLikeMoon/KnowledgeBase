目录
- [top](#top)
- [ps](#ps)
- [ss](#ss)
- [lsof](#lsof)
- [vmstat](#vmstat)
- [netstat](#netstat)
- [pidstat](#pidstat)
- [mpstat](#mpstat)
- [iostat](#iostat)
- [dstat](#dstat)
- [sar](#sar)
- [iotop](#iotop)
- [strace](#strace)
- [slabtop](#slabtop)
- [tcpdump](#tcpdump)
- [sysctl](#sysctl)
- [tar](#tar)


# top

*监控系统状况*

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

- top页面参数
```bash
?                             # 显示当前可输入的命令
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

- 标题含义
```bash
top                           # 当前时间,系统运行时间(up),当前登录用户数(user),系统负载(load average):1分钟 5分钟 15分钟
Tasks                         # 进程总数(total),正在运行进程数(running),睡眠进程数(sleeping),停止进程数(stopped),僵尸进程数(zombie)
Cpu                           # 用户空间占用CPU比(us),内核空间占用CPU比(sy),用户进程空间内改变过优先级的进程占用CPU比(ni),空闲CPU比(id),IO等待CPU比(wa),硬中断占用CPU比(hi),软中断占用CPU比(si),显示被虚拟CPU占用的时间(st)
Mem                           # 总物理内存(total),空闲内存量(free),已使用物理内存量(used),用于内核缓存的内存量(buff/cache)
Swap                          # 交换分区总量(total),空闲交换分区量(free),已使用交换分区量(used),缓冲的交换分区量(avail)
PID                           # 进程ID(PID),进程所有者(USER),优先级(PR),nice值:正高负低(NI),进程使用的虚拟内存量kb(VIRT),进程占用物理内存量kb(RES),共享内存量kb(SHR),进程状态:D不可中断 R运行 S睡眠 T跟踪 Z僵尸进程(S),上次更新到现在的CPU时间占用比(%CPU),进程使用物理内存比(%MEM),进程使用的CPU时间总计:单位1/100秒(TIME+),命令名称(COMMAND)
```

- 示例
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

# ps
*当前系统进程状态*

- 常用参数
```bash
-A                   # 列出所有进程
-e                   # 列出所有进程
-w                   # 列加宽显示
-aux                 # 显示所有进程的详细状态
-efl                 # 显示所有进程的完整信息
```

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

root@debian:~# ps -ef
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

# ss
*ss 用于转储套接字统计数据。它可以显示类似于 netstat 的信息。与其他工具相比，它能显示更多 TCP 和状态信息。*

- 参数
```bash
-h, --help          # 显示帮助信息
-V, --version       # 输出版本信息
-n, --numeric       # 不解析服务名称
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
          FAMILY := {inet|inet6|link|unix|netlink|vsock|tipc|xdp|help}
```

- 示例
```bash
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
```

# lsof
*lsof 是 List Open File 的缩写, 它主要用来获取被进程打开文件的信息，lsof命令可以查看所有已经打开了的文件，比如: 普通文件，目录，特殊的块文件，管道，socket套接字，设备，Unix域套接字等等，同时，它还可以结合 grep 以及 ps 命令进行更多的高级搜索*

- 示例
```bash
root@debian:~# lsof -i:22
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    1351 root    3u  IPv4  30588      0t0  TCP *:ssh (LISTEN)
sshd    1351 root    4u  IPv6  30599      0t0  TCP *:ssh (LISTEN)
sshd    1370 root    4u  IPv4  31789      0t0  TCP 172.21.100.222:ssh->172.21.100.201:52836 (ESTABLISHED)
sshd    1372 root    4u  IPv4  31794      0t0  TCP 172.21.100.222:ssh->172.21.100.201:52837 (ESTABLISHED)
```

- 标题含义解释
```bash
COMMAND                       进程名
PID                           进程ID
USER                          所属用户
FD                            文件描述符
          cwd                 当前目录
          txt                 文本文件
          rtd                 root目录
          mem                 内存映射文件

          0                   标准输出
          1                   标准输入
          2                   标准错误
                    u         表示读取和写入模式
                    r         表示只读模式
                    w         表示写入模式
                    -         表示处于未知状态,且未被锁定
TYPE                          文件类型
          DIR                 目录
          REG                 普通文件
          CHR                 字符
          a_inode             inode文件
          FIFO                管道或socket文件
          netlink             网络
          unkown              未知
DEVICE                        设备ID
SIZE/OFF                      进程大小
NODE                          文件inode号
NAME                          路径或链接
```

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
lsof name                                                  # 显示打开文件name的进程,即查找某个文件相关的进程
lsof +D                                                    # 递归显示目录及文件的信息
lsof +d                                                    # 显示目录的信息
```
# vmstat

*虚拟内存,进程,CPU活动监控*
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

- 标题说明
```bash
Procs:                  # 进程
r                       # 运行队列中进程数量(大于CPU核心数则会出现CPU瓶颈)
b                       # 等待IO的进程数量
Memory:                 # 内存(KB)
swpd                    # 使用虚拟内存大小
free                    # 可用内存大小
buff                    # 缓冲的内存大小
cache                   # 缓存的内存大小
Swap:                   # 交换分区(KB)
si                      # 每秒从交换分区写入内存的大小
so                      # 每秒写入交换分区的内存大小
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

- 示例
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

# netstat

```bash

```

# pidstat

*显示Linux进程的统计信息*
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

- 标题含义
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

- 示例
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

# mpstat

*显示CPU的性能统计信息*
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

- 标题含义
```bash
CPU                                 # CPU编号
%user                               # 用户态的CPU时间(%)，不包含nice值为负进程  (usr/total)*100
%nice                               # nice值为负进程的CPU时间(%)   (nice/total)*100
%sys                                # 内核时间(%)       (system/total)*100
%iowait                             # 硬盘IO等待时间(%) (iowait/total)*100
%irq                                # 硬中断时间(%)     (irq/total)*100
%soft                               # 软中断时间(%)     (softirq/total)*100
%idle                               # CPU除去等待磁盘IO操作外的因为任何原因而空闲的时间闲置时间(%) (idle/total)*100
```

- 示例
```bash
root@ubuntu:~# mpstat
Linux 5.15.0-118-generic (ubuntu)       08/15/2024      _x86_64_        (2 CPU)

04:17:11 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
04:17:11 PM  all    0.05    0.00    1.14    0.02    0.00    0.25    0.00    0.00    0.00   98.54
```

# iostat

*报告中央处理单元（CPU）统计信息以及设备和分区的输入/输出统计信息*
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

- 标题说明
```bash
avg-cpu:
%user                               # 显示在用户级（应用程序）执行期间的 CPU 使用百分比。
%nice                               # 显示在用户级（应用程序）以 nice 优先级执行期间的 CPU 使用百分比。
%system                             # 显示在系统级（内核）执行期间的 CPU 使用百分比。
%iowait                             # 显示 CPU 或多个 CPU 在系统有未完成的磁盘 I/O 请求期间空闲的时间百分比。
%steal                              # 显示虚拟 CPU 或多个虚拟 CPU 在虚拟化管理程序（Hypervisor）服务其他虚拟处理器期间被迫等待的时间百分比。
%idle                               # 显示 CPU 或多个 CPU 在系统没有未完成的磁盘 I/O 请求期间空闲的时间百分比。
Device:

```

- 示例
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

# dstat

*生成多种系统资源统计数据*
- 参数
```bash
-c, --cpu                               # 仅显示CPU统计信息
   -C 0,3,total                         #    include cpu0, cpu3 and total
-d, --disk                              # 仅显示磁盘统计信息
   -D total,hda                         #    include hda and total
-g, --page                              # 仅显示页统计信息
-i, --int                               # 仅显示中断统计信息
   -I 5,eth2                            #    include int5 and interrupt used by eth2
-l, --load                              # 仅显示负载统计信息
-m, --mem                               # 仅显示内存统计信息
-n, --net                               # 仅显示网络统计信息
   -N eth1,total                        #    include eth1 and total
-p, --proc                              # 仅显示进程统计信息
-r, --io                                # 仅显示IO统计信息 (I/O requests completed)
-s, --swap                              # 仅显示交换分区统计信息
   -S swap1,total                       #    include swap1 and total
-t, --time                              # 仅显示当前系统日期及时间
-T, --epoch                             # 仅显示时间计数器 (seconds since epoch)
-y, --sys                               # 仅显示系统统计信息
--aio                                   # enable aio stats
--fs, --filesystem                      # enable fs stats
--ipc                                   # enable ipc stats
--lock                                  # enable lock stats
--raw                                   # enable raw stats
--socket                                # 仅显示socket统计信息
--tcp                                   # 仅显示TCP统计信息
--udp                                   # 仅显示UDP统计信息
--unix                                  # 仅显示UNIX统计信息
--vm                                    # enable vm stats
--vm-adv                                # enable advanced vm stats
--zones                                 # enable zoneinfo stats
--list                                  # list all available plugins
--<plugin-name>                         # enable external plugin by name (see --list)
-a, --all                               # equals -cdngy (default)
-f, --full                              # automatically expand -C, -D, -I, -N and -S lists
-v, --vmstat                            # equals -pmgdsc -D total
--bits                                  # force bits for values expressed in bytes
--float                                 # force float values on screen
--integer                               # force integer values on screen
--bw, --black-on-white                  # change colors for white background terminal
--color                                 # 强制显示颜色
--nocolor                               # 关闭显示颜色
--noheaders                             # disable repetitive headers
--noupdate                              # disable intermediate updates
--output file                           # 显示内容输出为CSV格式文件
--profile                               # show profiling statistics when exiting dstat

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
```

# sar

*收集,汇报和保存系统活动信息*
- 参数
```bash
-A                                 # 显示所有统计数据
-B                                 # 显示内存分页的统计数据 [A_PAGE]
-b                                 # 显示IO和传输速率的统计数据[A_IO]
-d                                 # 显示块设备的统计数据 [A_DISK]
-F [ MOUNT ]                       # 显示指定文件系统的统计数据 [A_FS]
-H                                 # 大内存页的统计数据 [A_HUGE]
-I { <int_list> | SUM | ALL }      # 中断统计数据 [A_IRQ]
-m { <keyword> [,...] | ALL }      # 电源管理统计数据 [A_PWR_...]
        Keywords are:
        CPU     CPU instantaneous clock frequency
        FAN     Fans speed
        FREQ    CPU average clock frequency
        IN      Voltage inputs
        TEMP    Devices temperature
        USB     USB devices plugged into the system
-n { <keyword> [,...] | ALL }       # 网络统计数据 [A_NET_...]
        Keywords are:
        DEV     Network interfaces
        EDEV    Network interfaces (errors)
        NFS     NFS client
        NFSD    NFS server
        SOCK    Sockets (v4)
        IP      IP traffic      (v4)
        EIP     IP traffic      (v4) (errors)
        ICMP    ICMP traffic    (v4)
        EICMP   ICMP traffic    (v4) (errors)
        TCP     TCP traffic     (v4)
        ETCP    TCP traffic     (v4) (errors)
        UDP     UDP traffic     (v4)
        SOCK6   Sockets (v6)
        IP6     IP traffic      (v6)
        EIP6    IP traffic      (v6) (errors)
        ICMP6   ICMP traffic    (v6)
        EICMP6  ICMP traffic    (v6) (errors)
        UDP6    UDP traffic     (v6)
        FC      Fibre channel HBAs
        SOFT    Software-based network processing
-q [ <keyword> [,...] | PSI | ALL ] # 系统负载和系统压力统计数据
        Keywords are:
        LOAD    Queue length and load average statistics [A_QUEUE]
        CPU     Pressure-stall CPU statistics [A_PSI_CPU]
        IO      Pressure-stall I/O statistics [A_PSI_IO]
        MEM     Pressure-stall memory statistics [A_PSI_MEM]
-r [ ALL ]                         # 内存利用率统计 [A_MEMORY]
-S                                 # 交换分区利用率统计 [A_MEMORY]
-u [ ALL ]                         # CPU利用率统计 [A_CPU]
-v                                 # 内核表统计 [A_KTABLES]
-W                                 # 内外存对换统计数据 [A_SWAP]
-w                                 # 任务创建和系统切换统计 [A_PCSW]
-y                                 # TTY设备统计 [A_SERIAL]
-P [cpu_list|ALL]                  # 显示指定PID和进程名的CPU利用率统计
-o                                 # 将统计信息写入文件
-f                                 # 从文件中读取统计信息
```

- 示例
```bash
root@ubuntu:~# sar 1 1 
Linux 5.15.0-119-generic (ubuntu)       08/23/2024      _x86_64_        (2 CPU)

11:15:05 AM     CPU     %user     %nice   %system   %iowait    %steal     %idle
11:15:06 AM     all      0.00      0.00      1.99      0.00      0.00     98.01
Average:        all      0.00      0.00      1.99      0.00      0.00     98.01

```

# iotop

*简单的IO排列监视器*
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
-a, --accumulated                  # 显示累计 I/O 而不是带宽。在这种模式下，iotop 会显示自 iotop 启动以来进程的 I/O 数量。
-k, --kilobytes                    # 使用KB而非人为单位。这种模式在编写 iotop 的批处理模式脚本时非常有用。iotop 不会选择最合适的单位，而是以千字节为单位显示所有大小。
-t, --time                         # 每行添加时间戳
-q, --quiet                        # suppress some lines of header (implies --batch
--no-help                          # 显示帮助快捷键
```

- 示例
```bash
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

```

# strace

```bash

```

# slabtop

```bash

```

# tcpdump

```bash

```

# sysctl

```bash

```

# tar

*文件或目录打包解压工具*
- 参数
```bash
-c                                      # 这会创建一个存档文件。
-x                                      # 该选项提取了存档文件。
-f                                      # 指定归档文件的文件名。
-v                                      # 这将打印终端上任何tar操作的粗略信息。
-t                                      # 仅列出一个归档文件内的所有文件,不解压
-u                                      # 归档一个文件，然后把它添加到一个现有的归档文件中。
-r                                      # 追加文件至归档文件中
-z                                      # 使用gzip压缩创建一个tar文件
-j                                      # 使用bzip2压缩法创建一个存档文件
-W                                      # 选项验证一个归档文件。
-C                                      # 指定解压目录
--delete                                # 删除归档文件中的文件
```

- 示例
```bash
# 使用gzip格式压缩
tar -czvf sales.tar.gz  sales1.pdf sales2.pdf sales3.pdf
# 解压
tar -xzvf sales.tar.gz -C ./
```