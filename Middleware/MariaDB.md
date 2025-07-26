- [MariaDB 官网](#mariadb-官网)
- [MariaDB 仓库源](#mariadb-仓库源)
- [MariaDB 安装](#mariadb-安装)
- [MariaDB 数据库初始化](#mariadb-数据库初始化)
- [MariaDB 语法](#mariadb-语法)
  - [MariaDB CREATE](#mariadb-create)
  - [MariaDB GRANT](#mariadb-grant)
  - [MariaDB DROP](#mariadb-drop)
  - [MariaDB ALTER](#mariadb-alter)
  - [MariaDB UPDATE](#mariadb-update)
  - [MariaDB SHOW](#mariadb-show)
- [全新部署主从](#全新部署主从)
  - [主库配置](#主库配置)
  - [从库配置](#从库配置)
  - [数据同步测试](#数据同步测试)
- [现有数据一主一从](#现有数据一主一从)
  - [主库配置](#主库配置-1)
  - [从库配置](#从库配置-1)
- [级联复制](#级联复制)
  - [中间节点配置](#中间节点配置)
  - [last节点配置](#last节点配置)
- [双主复制](#双主复制)
- [半同步复制](#半同步复制)
  - [主节点配置](#主节点配置)
  - [从节点配置](#从节点配置)
- [复制过滤](#复制过滤)
  - [主库配置](#主库配置-2)
  - [从库配置](#从库配置-2)
- [GTID复制](#gtid复制)
  - [主库配置](#主库配置-3)
  - [从库配置](#从库配置-3)
- [MAriaDB 操作](#mariadb-操作)

# MariaDB 官网

[https://mariadb.org/download/?t=repo-config](https://mariadb.org/download/?t=repo-config)

# MariaDB 仓库源

- 安装官方密钥
```bash
sudo apt-get install apt-transport-https curl
sudo curl -o /etc/apt/trusted.gpg.d/mariadb_release_signing_key.asc 'https://mariadb.org/mariadb_release_signing_key.asc'
sudo sh -c "echo 'deb https://mirrors.aliyun.com/mariadb/repo/10.9/ubuntu jammy main' >>/etc/apt/sources.list"
```

- Ubuntu22.04

`/etc/apt/sources.list.d/mariadb.list`
```bash
# MariaDB 10.9 repository list - created 2022-12-18 07:52 UTC
# https://mariadb.org/download/
deb https://mirrors.aliyun.com/mariadb/repo/10.9/ubuntu jammy main
# deb-src https://mirrors.aliyun.com/mariadb/repo/10.9/ubuntu jammy main
deb https://mirrors.aliyun.com/mariadb/repo/10.9/ubuntu jammy main/debug'
```

- Ubuntu24.04

`/etc/apt/sources.list.d/mariadb.sources`

```bash
# MariaDB 11.4 repository list - created 2025-02-19 04:24 UTC
# https://mariadb.org/download/
X-Repolib-Name: MariaDB
Types: deb
# deb.mariadb.org is a dynamic mirror if your preferred mirror goes offline. See https://mariadb.org/mirrorbits/ for details.
# URIs: https://deb.mariadb.org/11.4/ubuntu
URIs: https://mirror.citrahost.com/mariadb/repo/11.4/ubuntu
Suites: noble
Components: main main/debug
Signed-By: /etc/apt/keyrings/mariadb-keyring.pgp
```

# MariaDB 安装

```bash
sudo apt-get update
sudo apt-get install mariadb-server mariadb-client
```

# MariaDB 数据库初始化

```bash
root@database:~# mariadb-secure-installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

# MariaDB 语法
## MariaDB CREATE
## MariaDB GRANT
## MariaDB DROP
## MariaDB ALTER
## MariaDB UPDATE
## MariaDB SHOW

# 全新部署主从

## 主库配置

```bash
# 修改mariadb配置
root@master:# vim /etc/mysql/my.cnf
[mysqld] # 指定服务端配置
server-id=160 # 指定server-id,唯一
binlog-format = ROW
sync_binlog = 1                         # 每次事务提交时同步binlog
binlog-row-image = FULL                 # 确保行级别复制包含所有列
log_error = /var/log/mariadb/mariadb-err.log
log_bin=/data/mariadb/logbin/mariadb-bin # 指定二进制文件位置

# 创建目录
root@master:# mkdir -pv /data/mariadb/logbin

# 赋予mysql目录权限
root@master:# chown -R mysql:mysql /data/mariadb

# 重启maraidb服务
root@master:# systemctl restart mariadb


# 创建同步账户
MariaDB [(none)]> create user 'repluser'@'%' identified by 'ubuntu';
Query OK, 0 rows affected (0.001 sec)
# 赋予同步权限
MariaDB [(none)]> grant replication slave on *.* to 'repluser'@'%';
Query OK, 0 rows affected (0.000 sec)
# 查看master日志大小
MariaDB [(none)]> show master logs;
+------------------+-----------+
| Log_name         | File_size |
+------------------+-----------+
| mariadb-bin.000001 |       647 |
+------------------+-----------+
1 row in set (0.000 sec)
# 查看master状态
MariaDB [(none)]> show master status;
+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mariadb-bin.000001 |      647 |              |                  |
+------------------+----------+--------------+------------------+
1 row in set (0.000 sec)
```

## 从库配置

```bash
# 修改mariadb配置
root@slave:# vim /etc/mysql/my.cnf
[mysqld] # 指定服务端配置
server-id=161 # 指定server-id,唯一
read-only # 只读模式
log_bin=/data/mariadb/logbin/mariadb-bin # 指定二进制文件位置

# 创建目录
root@master:# mkdir -pv /data/mariadb/logbin

# 赋予mysql目录权限
root@master:# chown -R mysql:mysql /data/mariadb

# 重启maraidb服务
root@master:# systemctl restart mariadb

# 查看节点状态
MariaDB [(none)]> show slave status;
Empty set (0.00 sec)

# 配置主从同步
MariaDB [(none)]> change master to MASTER_HOST='172.21.100.160',MASTER_USER='repluser',MASTER_PASSWORD='ubuntu',MASTER_PORT=3306,MASTER_LOG_FILE='mariadb-bin.000001',MASTER_LOG_POS=647; # MASTER_LOG_POS的数值在master节点查询时的mariadb-bin对应的file_size值
Query OK, 0 rows affected, 1 warning (0.002 sec)

# 查看slave节点信息
MariaDB [(none)]> show slave status\G
*************************** 1. row ***************************
                Slave_IO_State:
                   Master_Host: 172.21.100.160
                   Master_User: repluser
                   Master_Port: 3306
                 Connect_Retry: 60
               Master_Log_File: mariadb-bin.000001
           Read_Master_Log_Pos: 157
                Relay_Log_File: mysqld-relay-bin.000001
                 Relay_Log_Pos: 4
         Relay_Master_Log_File: mariadb-bin.000001
              Slave_IO_Running: No
             Slave_SQL_Running: No
               Replicate_Do_DB:
           Replicate_Ignore_DB:
            Replicate_Do_Table:
        Replicate_Ignore_Table:
       Replicate_Wild_Do_Table:
   Replicate_Wild_Ignore_Table:
                    Last_Errno: 0
                    Last_Error:
                  Skip_Counter: 0
           Exec_Master_Log_Pos: 157
               Relay_Log_Space: 256
               Until_Condition: None
                Until_Log_File:
                 Until_Log_Pos: 0
            Master_SSL_Allowed: Yes
            Master_SSL_CA_File:
            Master_SSL_CA_Path:
               Master_SSL_Cert:
             Master_SSL_Cipher:
                Master_SSL_Key:
         Seconds_Behind_Master: NULL
 Master_SSL_Verify_Server_Cert: Yes
                 Last_IO_Errno: 0
                 Last_IO_Error:
                Last_SQL_Errno: 0
                Last_SQL_Error:
   Replicate_Ignore_Server_Ids:
              Master_Server_Id: 0
                Master_SSL_Crl:
            Master_SSL_Crlpath:
                    Using_Gtid: No
                   Gtid_IO_Pos:
       Replicate_Do_Domain_Ids:
   Replicate_Ignore_Domain_Ids:
                 Parallel_Mode: optimistic
                     SQL_Delay: 0
           SQL_Remaining_Delay: NULL
       Slave_SQL_Running_State:
              Slave_DDL_Groups: 0
Slave_Non_Transactional_Groups: 0
    Slave_Transactional_Groups: 0
          Replicate_Rewrite_DB:
                Connects_Tried: 0
            Master_Retry_Count: 100000
1 row in set (0.006 sec)

# 启动同步
MariaDB [(none)]> start slave;
Query OK, 0 rows affected (0.000 sec)

# 查看slave节点信息
MariaDB [(none)]> show slave status\G
*************************** 1. row ***************************
                Slave_IO_State: Waiting for master to send event
                   Master_Host: 172.21.100.160
                   Master_User: repluser
                   Master_Port: 3306
                 Connect_Retry: 60
               Master_Log_File: mariadb-bin.000003
           Read_Master_Log_Pos: 342
                Relay_Log_File: mysqld-relay-bin.000003
                 Relay_Log_Pos: 641
         Relay_Master_Log_File: mariadb-bin.000003
              Slave_IO_Running: Yes
             Slave_SQL_Running: Yes
               Replicate_Do_DB:
           Replicate_Ignore_DB:
            Replicate_Do_Table:
        Replicate_Ignore_Table:
       Replicate_Wild_Do_Table:
   Replicate_Wild_Ignore_Table:
                    Last_Errno: 0
                    Last_Error:
                  Skip_Counter: 0
           Exec_Master_Log_Pos: 342
               Relay_Log_Space: 1250
               Until_Condition: None
                Until_Log_File:
                 Until_Log_Pos: 0
            Master_SSL_Allowed: Yes
            Master_SSL_CA_File:
            Master_SSL_CA_Path:
               Master_SSL_Cert:
             Master_SSL_Cipher:
                Master_SSL_Key:
         Seconds_Behind_Master: 0
 Master_SSL_Verify_Server_Cert: Yes
                 Last_IO_Errno: 0
                 Last_IO_Error:
                Last_SQL_Errno: 0
                Last_SQL_Error:
   Replicate_Ignore_Server_Ids:
              Master_Server_Id: 160
                Master_SSL_Crl:
            Master_SSL_Crlpath:
                    Using_Gtid: No
                   Gtid_IO_Pos:
       Replicate_Do_Domain_Ids:
   Replicate_Ignore_Domain_Ids:
                 Parallel_Mode: optimistic
                     SQL_Delay: 0
           SQL_Remaining_Delay: NULL
       Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
              Slave_DDL_Groups: 0
Slave_Non_Transactional_Groups: 0
    Slave_Transactional_Groups: 0
          Replicate_Rewrite_DB:
                Connects_Tried: 1
            Master_Retry_Count: 100000
1 row in set (0.001 sec)

# 查看从节点线程
MariaDB [(none)]> show processlist\G
*************************** 1. row ***************************
      Id: 34
    User: root
    Host: localhost
      db: NULL
 Command: Query
    Time: 0
   State: starting
    Info: show processlist
Progress: 0.000

```

## 数据同步测试

```bash
MariaDB [(none)]> create database db1;
Query OK, 1 row affected (0.000 sec)

MariaDB [(none)]> use db1;
Database changed

MariaDB [db1]> create table `tb1` ( `id` int unsigned not null auto_increment, `name` varchar(20) not null,primary key (`id`) ) ENGINE=InnoDB;
Query OK, 0 rows affected (0.001 sec)

MariaDB [db1]> insert into tb1 (name) values ('xiaolong'),('mask'),('ruoma');
Query OK, 3 rows affected (0.001 sec)
Records: 3  Duplicates: 0  Warnings: 0

```

# 现有数据一主一从

## 主库配置
```bash
# 修改mariadb配置
root@master:# vim /etc/mysql/my.cnf
[mysqld] # 指定服务端配置
server-id=160 # 指定server-id,唯一
log_bin=/data/mariadb/logbin/mariadb-bin # 指定二进制文件位置

# 创建目录
root@master:# mkdir -pv /data/mariadb/logbin

# 赋予mysql目录权限
root@master:# chown -R mysql:mysql /data/mariadb

# 查看数据库现有日志
MariaDB [(none)]> show master logs;
+------------------+-----------+
| Log_name         | File_size |
+------------------+-----------+
| mariadb-bin.000001 |       375 |
| mariadb-bin.000002 |      3176 |
| mariadb-bin.000003 |       342 |
+------------------+-----------+
3 rows in set (0.000 sec)
# 重置二进制文件
MariaDB [(none)]> reset master;
Query OK, 0 rows affected (0.002 sec)
# 查看数据库日志
MariaDB [(none)]> show master logs;
+------------------+-----------+
| Log_name         | File_size |
+------------------+-----------+
| mariadb-bin.000001 |       328 |
+------------------+-----------+
1 row in set (0.000 sec)

# 全量备份的方式导出所有已有的数据,并刷新二进制日志
root@master:~# mariadb-dump -A -F --master-data=1 --single-transaction > master.sql

# 二进制文件被刷新
MariaDB [(none)]> show master logs;
+------------------+-----------+
| Log_name         | File_size |
+------------------+-----------+
| mariadb-bin.000001 |       375 |
| mariadb-bin.000002 |       371 |
+------------------+-----------+
2 rows in set (0.000 sec)

# 创建账户并授权
MariaDB [(none)]> create user 'repluser'@'%' identified by 'ubuntu';
Query OK, 0 rows affected (0.001 sec)
# 赋予同步权限
MariaDB [(none)]> grant replication slave on *.* to 'repluser'@'%';
Query OK, 0 rows affected (0.000 sec)

# 将备份文件传至从节点
root@master:~# scp master.sql ubuntu@172.21.100.161:/home/ubuntu
```

## 从库配置
```bash
# 修改mariadb配置
root@slave:# vim /etc/mysql/my.cnf
[mysqld] # 指定服务端配置
server-id=161 # 指定server-id,唯一
read-only # 只读模式
log_bin=/data/mariadb/logbin/mariadb-bin # 指定二进制文件位置

# 创建目录
root@master:# mkdir -pv /data/mariadb/logbin

# 赋予mysql目录权限
root@master:# chown -R mysql:mysql /data/mariadb

# 重启maraidb服务
root@master:# systemctl restart mariadb

# 修改备份文件内容,修改主库信息
root@master:# vim master.sql
...
CHANGE MASTER TO MASTER_HOST='172.21.100.160',MASTER_USER='repluser',MASTER_PASSWORD='ubuntu',MASTER_PORT=3306,MASTER_LOG_FILE='mysql-bin.000002', MASTER_LOG_POS=371;
...

# 停止二进制日志写入
MariaDB [(none)]> set sql_log_bin=0;

# 导入master.sql的数据
MariaDB [(none)]> source /home/ubuntu/master.sql;

# 查看从节点状态
MariaDB [(none)]> show slave status;

# 启动同步线程
MariaDB [(none)]> start slave;

# 查看从节点状态
MariaDB [(none)]> show slave status;

# 恢复二进制文件
MariaDB [(none)]> set sql_log_bin=1;

```

说明: 
如遇到事务同步错误, 执行事务提交操作或者事务回滚操作

```bash
# 事务提交
MariaDB [(none)]> COMMIT;
# 事务回滚
MariaDB [(none)]> ROLLBACK;
```

# 级联复制

## 中间节点配置

主从复制架构中, 从节点从中继日志中读取到数据写入数据库后, 该数据并不会写入到从节点的二进制日志中, 但是在级联同步架构中, 有一个中间节点的角色, 该节点从主节点中同步数据, 并充当其它节点的数据源, 所以在此情况下, 我们需要保证中间节点从主节点中同步过来的数据, 同样也要写二进制日志, 否则后续节点无法获取数据.

在此架构中, 中间节点要开启 log_slave_updates 选项, 保证中间节点复制过来的数据也能写入二进制日志, 为其它节点提供数据源.

```bash
MariaDB [(none)]> select @@log_slave_updates;
+---------------------+
| @@log_slave_updates |
+---------------------+
| 1                   |
+---------------------+
1 row in set, 1 warning (0.00 sec)

root@master:# vim /etc/mysql/my.cnf
......
[mysqld]
server-id=183
read-only=on
log_slave_updates=on
log-bin=/data/mysql/logbin/mysql-bin

root@master:# systemctl restart mariadb

#中间节点处于同步状态
MariaDB [(none)]> show slave status\G

#导出中间节点数据
root@master:# mariadb-dump -A -F --single-transaction --master-data=1 > middle-all.sql

#拷贝至last节点
root@master:# scp middle-all.sql ubuntu@10.0.0.186:/home/ubuntu

```

## last节点配置

```bash
# 清除所有数据
root@master:# rm -rf /var/lib/mysql/*

# 修改mariadb配置
root@last:# vim /etc/mysql/my.cnf
[mysqld]
server-id=186
read-only=on
log-bin=/data/mariadb/logbin/mariadb-bin

# 修改备份文件
root@last:# vim middle-all.sql
CHANGE MASTER TO  
MASTER_HOST='10.0.0.183',  
MASTER_USER='repluser',  
MASTER_PASSWORD='123456',  
MASTER_PORT=3306,  
MASTER_LOG_FILE='mysql-bin.000004',  
MASTER_LOG_POS=157;

# 启动服务
root@last:# systemctl restart mariadb

# 临时关闭二进制日志
MariaDB [(none)]> set sql_log_bin=0;
MariaDB [(none)]> select @@sql_log_bin;
+---------------------+
| @@sql_log_bin        |
+---------------------+
| 0                   |
+---------------------+

# 导入备份数据
MariaDB [(none)]> source /home/ubuntu/middle-all.sql;

# 恢复二进制日志
MariaDB [(none)]> set sql_log_bin=1;
MariaDB [(none)]> show slave status\G
MariaDB [(none)]> start slave;
MariaDB [(none)]> show slave status\G

```

# 双主复制

双主模型中, 两个节点互为主备, 两个节点都需要开启二进制日志, 都有写权限

```bash
# master1配置
root@master1:# vim /etc/mysql/my.cnf
[mysqld]
server-id=160
binlog-format=row
log-bin=/data/mariadb/logbin/mariadb-bin

# master2配置
root@master2:# vim /etc/mysql/my.cnf
[mysqld]
server-id=161
log-bin=/data/mariadb/logbin/mariadb-bin

# 启动master2服务
root@master2:# systemctl restart mariadb

# master2查看slave状态
MariaDB [(none)]> show slave status\G

# 查看master2的bin-log
MariaDB [(none)]> show master logs;

# 查看master1配置
MariaDB [(none)]> show master status;

# master1创建同步账户
root@master1:# create user 'repluser'@'%' identified by 'ubuntu';
Query OK, 0 rows affected (0.000 sec)
# master1创建同步权限
root@master1:# grant replication slave on *.* to 'repluser'@'%';
Query OK, 0 rows affected (0.000 sec)

# 配置master1同步信息
MariaDB [(none)]> CHANGE MASTER TO  MASTER_HOST='10.0.0.161',MASTER_USER='repluser',MASTER_PASSWORD='ubuntu',MASTER_PORT=3306,MASTER_LOG_FILE='mysql-bin.000002', MASTER_LOG_POS=371;

# 启动同步线程
MariaDB [(none)]> start slave;
Query OK, 0 rows affected (0.000 sec)
# 查看master1状态
MariaDB [(none)]> show slave status\G
```

说明: 
双主架构在实际生产环境中, 并不会配置为两个节点都去写数据, 前端应用只会写一个节点, 另一个节点作为备份节点, 如果当前使用的节点出问题, 则IP地址会立即转移到另一个节点上, 起到一个高可用的作用, 此时, 如果有slave节点, 在 slave 上要重新执行同步操作.

在双主架构中, 如果同时在两个节点执行写操作, 就可能会导致数据冲突, 从而影响主从同步.

```bash
# 双主出现错误, master1执行
# 停止主从
MariaDB [(none)]> stop slave;

# 跳过错误事件
MariaDB [(none)]> set global sql_slave_skip_counter=1;

# 再次开启主从
MariaDB [(none)]> start slave;

# 查看同步状态
MariaDB [(none)]> show slave status\G

# master2执行
MariaDB [(none)]> stop slave;

# 跳过错误事件
MariaDB [(none)]> set global sql_slave_skip_counter=1;

# 再次启动主从
MariaDB [(none)]> start slave;

# 查看同步状态
MariaDB [(none)]> show slave status\G
```

说明: 
#除了使用 `set global sql_slave_skip_counter=N` 忽略错误个数之外，也可以用忽略指定错误编号的方式来处理错误#`slave_skip_errors=N|ALL` 用来忽略指定的错误编号，但其是服务器选项，要写配置文件后重启服务
#`Last_Errno: 1062，Last_SQL_Errno: 1062` 这两个字段的值就是错误编号

```bash
[mysqld]
slave_skip_errors=N|ALL
```

# 半同步复制

当客户端程序向主节点中写入数据后, 主节点中的数据落盘, 写入binlog日志, 然后将binlog日志中的新事件发送给从节点, 等待所有节点中有一个从节点返回同步成功之后, 主节点就向客户端返回写入成功. 此复制策略尽可能保证至少有一个从节点有同步数据, 也尽可能早的向客户端返回写入状态.

## 主节点配置

```bash
# 查看master节点插件列表
MariaDB [(none)]> show plugins;

# master节点安装semisync_master插件
MariaDB [(none)]> INSTALL PLUGIN rpl_semi_sync_master SONAME 'semisync_master.so';

# 查看master节点插件列表
MariaDB [(none)]> show plugins;

# 查看mysql的plugin表的内容
MariaDB [(none)]> select * from mysql.plugin;

# 查看master插件状态
MariaDB [(none)]> select @@rpl_semi_sync_master_enabled;

# 修改master配置文件
root@master1:# vim /etc/mysql/my.cnf
[mysqld]
server-id=160
rpl_semi_sync_master_enabled=on # 启用半同步复制
rpl_semi_sync_master_timeout=1000 # 设置超时时间为1秒
rpl_semi_sync_master_wait_for_slave_count=1 # 设置等待从节点数量为1
rpl_semi_sync_master_wait_point=after_sync # 当前同步策略
log-bin=/data/mariadb/logbin/mariadb-bin

# 重启maraidb服务
root@master1:# systemctl restart mariadb

# 查看master插件状态
MariaDB [(none)]> select @@rpl_semi_sync_master_enabled;

# 查看master节点半同步配置
MariaDB [(none)]> show global variables like '%semi%';
+---------------------------------------+--------------+
| Variable_name                         | Value        |
+---------------------------------------+--------------+
| rpl_semi_sync_master_enabled          | ON           |
| rpl_semi_sync_master_timeout          | 10000        |
| rpl_semi_sync_master_trace_level      | 32           |
| rpl_semi_sync_master_wait_no_slave    | ON           |
| rpl_semi_sync_master_wait_point       | AFTER_COMMIT |
| rpl_semi_sync_slave_delay_master      | OFF          |
| rpl_semi_sync_slave_enabled           | OFF          |
| rpl_semi_sync_slave_kill_conn_timeout | 5            |
| rpl_semi_sync_slave_trace_level       | 32           |
+---------------------------------------+--------------+
9 rows in set (0.001 sec)

# 查看master节点半同步状态
MariaDB [(none)]> show global status like '%semi%';
+--------------------------------------------+-------+
| Variable_name                              | Value |
+--------------------------------------------+-------+
| Rpl_semi_sync_master_clients               | 0     |
| Rpl_semi_sync_master_get_ack               | 0     |
| Rpl_semi_sync_master_net_avg_wait_time     | 0     |
| Rpl_semi_sync_master_net_wait_time         | 0     |
| Rpl_semi_sync_master_net_waits             | 0     |
| Rpl_semi_sync_master_no_times              | 0     |
| Rpl_semi_sync_master_no_tx                 | 0     |
| Rpl_semi_sync_master_request_ack           | 0     |
| Rpl_semi_sync_master_status                | ON    |
| Rpl_semi_sync_master_timefunc_failures     | 0     |
| Rpl_semi_sync_master_tx_avg_wait_time      | 0     |
| Rpl_semi_sync_master_tx_wait_time          | 0     |
| Rpl_semi_sync_master_tx_waits              | 0     |
| Rpl_semi_sync_master_wait_pos_backtraverse | 0     |
| Rpl_semi_sync_master_wait_sessions         | 0     |
| Rpl_semi_sync_master_yes_tx                | 0     |
| Rpl_semi_sync_slave_send_ack               | 0     |
| Rpl_semi_sync_slave_status                 | OFF   |
+--------------------------------------------+-------+
18 rows in set (0.001 sec)

```

## 从节点配置

```bash
# 查看slave节点插件列表
MariaDB [(none)]> show plugins;

# slave节点安装semisync_slave插件
MariaDB [(none)]> INSTALL PLUGIN rpl_semi_sync_slave SONAME 'semisync_slave.so';

# 查看slave节点插件列表
MariaDB [(none)]> show plugins;

# 查看mysql的plugin表的内容
MariaDB [(none)]> select * from mysql.plugin;

# 查看slave插件状态
MariaDB [(none)]> select @@rpl_semi_sync_slave_enabled;

# 修改slave配置文件
root@master1:# vim /etc/mysql/my.cnf
[mysqld]
server-id=160
read-only
rpl_semi_sync_slave_enabled # 启用半同步复制
log-bin=/data/mariadb/logbin/mariadb-bin

# 重启maraidb服务
root@master1:# systemctl restart mariadb

# 查看slave插件状态
MariaDB [(none)]> select @@rpl_semi_sync_slave_enabled;

```

# 复制过滤

主库配置同步数据库白名单及黑名单,减少数据量和网络存储IO,因数据缺失恢复时不全面
从库配置同步数据库白名单及黑名单,网络存储IO负载重

## 主库配置

```bash
vim /etc/mysql/my.cnf

[mysqld]
log-bin=/data/mariadb/logbin/mariadb-bin
binlog-format=row
# 仅需要同步的数据库,有多个库则多行
binlog-do-db=db1
# 除不需要同步的数据库,其余数据库均同步
binlog-ignore-db=db2
```

## 从库配置

```bash
vim /etc/mysql/my.cnf

[mysqld]
log-bin=/data/mariadb/logbin/mariadb-bin
binlog-format=row
# 需要同步的库
replcate-do-db=db1
# 需要同步的表
replcate-do-table=db1.tb1
# 不需要同步的库
replcate-ignore-db=db2
# 不需要同步的表
replcate-ignore-table=db1.tb2
# 需要同步的库,支持通配符,每行一个规则,多规则多行
replcate-wild-do-table=db1.tb%
# 不需要同步的库,支持通配符,每行一个规则,多规则多行
replcate-wild-ignore-table=db1.tb%

```

# GTID复制

GTID是已提交的事务编号, 由当前server-uuid和transacton-id组成, 每个事务transacton-id在当前节点唯一

## 主库配置
```bash
# 配置文件
vim /etc/mysql/my.cnf

[mysqld]
server_id              = 11              # 唯一
report_host            = primary
log_error              = /var/log/mariadb/mariadb.err
# 二进制日志
log_bin                = mariadb-bin
# log_bin_index          = mariadb-bin.index
binlog_format          = ROW
sync_binlog            = 1
binlog_checksum        = CRC32
# GTID
gtid_domain_id         = 1
gtid_strict_mode       = ON
enforce_gtid_consistency = ON
# log_slave_updates      = ON
# 半同步
rpl_semi_sync_master_enabled    = ON
rpl_semi_sync_master_timeout    = 10000    #10s
rpl_semi_sync_master_wait_point = AFTER_SYNC   #崩溃零丢失
# 其他一致性
innodb_flush_log_at_trx_commit  = 1
replica_preserve_commit_order   = ON


# 创建复制账号
MariaDB [(none)]> CREATE USER 'repl'@'10.%' IDENTIFIED BY 'Str0ng!';
MariaDB [(none)]> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'10.%';
MariaDB [(none)]> FLUSH PRIVILEGES;

# 备份主库
mariadb-backup --backup --target-dir=/backup/full --user=root --password
mariadb-backup --prepare --target-dir=/backup/full
rsync -a /backup/full/  replica1:/var/lib/mysql/

# 查看主库当前binlog信息
SHOW MASTER STATUS\G

# 查看半同步复制状态
SHOW GLOBAL STATUS LIKE 'Rpl_semi_sync_master_status';

# 查看主库半同步是否处于激活状态
SHOW GLOBAL STATUS LIKE 'Rpl_semi_sync_master_status'\G

# 主库即时查看gtid-pos, 从库设置
SELECT @@GLOBAL.gtid_current_pos;

# 验证 GTID 同步是否一致（主从两边应相等）
SELECT @@gtid_current_pos, @@gtid_slave_pos\G

``` 
## 从库配置
```bash
vim /etc/mysql/my.cnf

[mysqld]
server_id              = 161
report_host            = replica1
read_only              = ON
# super_read_only        = ON
log_bin                = mariadb-bin
log_slave_updates      = ON
gtid_domain_id         = 1
gtid_strict_mode       = ON
rpl_semi_sync_slave_enabled = ON
replica_preserve_commit_order = ON
# skip_slave_start       = ON     # 让我们先导入备份再启动

# 建立复制信息
-- rsync 下来的 xtrabackup_binlog_info 中已包含 GTID
SET GLOBAL gtid_slave_pos = '0-1-<next>';
CHANGE MASTER TO
  MASTER_HOST='10.0.0.11',
  MASTER_USER='repl',
  MASTER_PASSWORD='Str0ng!',
  MASTER_USE_GTID=slave_pos;   -- GTID 自动定位

START SLAVE;
```

# MAriaDB 操作

- 查看用户地址信息
```bash
MariaDB [(none)]> SELECT User, Host FROM mysql.user;
+-------------+-----------+
| User        | Host      |
+-------------+-----------+
| mariadb.sys | localhost |
| mysql       | localhost |
| root        | localhost |
| zabbix      | localhost |
+-------------+-----------+
4 rows in set (0.000 sec)
```

- 查看数据库正在使用的参数
```bash
root@zxbnode01:/# mariadbd --print-defaults
mariadbd would have been started with the following arguments:
--socket=/run/mysqld/mysqld.sock --pid-file=/run/mysqld/mysqld.pid --basedir=/usr --general_log_file=/var/log/mysql/mysql.log --general_log=1 --expire_logs_days=10 --character-set-server=utf8mb4 --collation-server=utf8mb4_general_ci --plugin_load_add=provider_bzip2 --provider_bzip2=force_plus_permanent --plugin_load_add=provider_lz4 --provider_lz4=force_plus_permanent --plugin_load_add=provider_lzma --provider_lzma=force_plus_permanent --plugin_load_add=provider_lzo --provider_lzo=force_plus_permanent --plugin_load_add=provider_snappy --provider_snappy=force_plus_permanent
```

- 查看用户权限
```bash
MariaDB [(none)]> show grants for zabbix@localhost;
+---------------------------------------------------------------------------------------------------------------+
| Grants for zabbix@localhost                                                                                   |
+---------------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `zabbix`@`localhost` IDENTIFIED BY PASSWORD '*DEEF4D7D88CD046ECA02A80393B7780A63E7E789' |
| GRANT ALL PRIVILEGES ON `zabbix`.* TO `zabbix`@`localhost`                                                    |
+---------------------------------------------------------------------------------------------------------------+
2 rows in set (0.000 sec)
```

- 设置远程访问

`/etc/mysql/mariadb.conf.d/50-server.cnf`
```bash
bind-address            = 0.0.0.0
```

```bash
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'zabbix' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'172.21.100.%' IDENTIFIED BY 'zabbix' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

```bash
alter user 'root'@'%' identified by 'zabbix';
```
