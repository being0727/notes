redis安装和启动



```shel
#查看所有端口连接情况
ss -tanl 
#查看redis运行情况
ps -ef|grep redis
#杀死某个进行
#停止某个端口的redis-service
redis-cli -h 127.0.0.1 -p 6381  shutdown
#
cd /opt/redis 
```



实时查看日志 

tail的使用

```shell
tail -f app.log
```

1. 命令格式;
   tail[必要参数][选择参数][文件]2. 命令功能：
   用于显示指定文件末尾内容，不指定文件时，作为输入信息进行处理。常用查看日志文件。
2. 命令参数：
   -f 循环读取
   -q 不显示处理信息
   -v 显示详细的处理信息
   -c<数目> 显示的字节数
   -n<行数> 显示行数
   –pid=PID 与-f合用,表示在进程ID,PID死掉之后结束.
   -q, –quiet, –silent 从不输出给出文件名的首部
   -s, –sleep-interval=S 与-f合用,表示在每次反复的间隔休眠S秒

# 主从模式—哨兵模式

## 主从复制创建

​	redis-server --port <端口> --slaveof  **< master-ip >  <master-port>**

```shell
redis-server --6380 --slaveof 127.0.0.1 6380
```

slaveof host port 命令，将当前服务器状态从master修改为别的服务器的slave

```shell
redis>slaveof 192.168.1.1 6379  #将服务器转换成Slave
redis>slaveof NO ONE # 将服务器重新恢复到Master,不会丢弃已同步的数据
```



配置方式：启动时，服务器读取配置文件，并自动成为制定服务器的从服务器

```shell
#slaveof <masterip> <masterport>
slaveof 127.0.0.1 6379
```

## 主从复制问题

1.一个Master可以有多个Slaves

2.slave下线，只是读请求的处理性能下降

3.Master下线，写请求无法执行

4.其中一台Slave使用SLAVEOF no one 命令成为了Master,其他Slaves执行SLAVEOF命令指向这个新的Master,从他这里同步数据

**以上过程是手动的,能够实现自动，就需要Sentinel哨兵，实现故障转移Failover操作**

## 解决问题：

### 使用redis-sentinel

主从复制+哨兵Sentinel只解决了读性能和高可用问题，但没有解决写的问题

```shell
#复制文件
cp sentinel.conf /etc/redis/sentinel_26379.conf
#修改文件
vim /etc/redis/sentinel_26379.conf
#添加日志记录的位置
logfile "/var/log/redis_sentinel_26379.log"
#启动redis-sentinel 
redis-sentinel /etc/redis/sentinel_26379.conf
```



# Redis Twemproxy

## Twemproxy的说明

![Alt text](.\img\Twemproxy说明.jpg)

Twemproxy的配置：（类似yml文件）





# Redis 原生3.x集群模式

```shell
#创建集群需要的目录并修改配置文件redis_cluster.conf
#创建6个
mkdir -p /usr/local/cluster
mkdir -p /usr/local/cluster/7000
mkdir -p /usr/local/cluster/7001
mkdir -p /usr/local/cluster/7002
mkdir -p /usr/local/cluster/7003
mkdir -p /usr/local/cluster/7004
mkdir -p /usr/local/cluster/7005
vim /usr/local/redis3.0/redis.conf
port 7000
daemonize yes
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
appendonly yes

cp -rf /usr/local/redis3.0/redis.conf /usr/local/cluster/7000/
cp -rf /usr/local/redis3.0/redis.conf /usr/local/cluster/7001/
cp -rf /usr/local/redis3.0/redis.conf /usr/local/cluster/7002/
#启动6个redis
cd /usr/local/cluster
redis-server 7000/redis.conf
#创建redis集群
redis-trib.rb  create --replicas 1 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005

#如果缺少ruby 
yum install ruby rubygems -y

#安装redis-3.2.1.gem
需要手工下载并安装：
wget https://rubygems.global.ssl.fastly.net/gems/redis-3.2.1.gem
gem install -l ./redis-3.2.1.gem

#再次执行该命令，创建redis集群
redis-trib.rb  create --replicas 1 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005

```





​	













