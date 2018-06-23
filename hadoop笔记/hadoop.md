# 环境搭建

## 网络同步时间

```shell
ntpdate cn.pool.ntp.org
```

## 设置主机名

```shell
#1.
hostnamectl set-hostname  node-1
#2.
vim /etc/hostname 
reboot #修改后重启
```

## 修改主机IP地址

## 配置IP、主机名映射

```shell
cat /etc/hosts
#增加node-1等主机映射
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.17.132 node-1
192.168.17.133 node-2
192.168.17.134 node-3

```

## 配置免密登录

```shell
#1.生成
ssh-keygen -t ras  #四个回车键

Enter file in which to save the key (/root/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:/umjoUF5dgNgJt8HcWUslkXs8+X7kz50TADPY593nek root@node-1
The key's randomart image is:
+---[RSA 2048]----+
|    . + o..B*.   |
|     = o o+.oo.  |
|      . o..o  =. |
|       . o  o. oB|
|      o S o  o O=|
|     . + . .  o.*|
|      . o     .E+|
|       o o..   = |
|      . .o+.  ..=|
+----[SHA256]-----+

#2.复制到node-1,node-2 ,node-3

ssh-copy-id node-1
```

## 关闭防火墙

## jdk环境安装

## hadoop下载和安装

1.

```shell
#/root目录下新建几个目录，复制粘贴执行下面的命令：
mkdir  /root/hadoop  
mkdir  /root/hadoop/tmp  
mkdir  /root/hadoop/var  
mkdir  /root/hadoop/dfs  
mkdir  /root/hadoop/dfs/name  
mkdir  /root/hadoop/dfs/data  

#修改五个配置文件
#注意hadoop-2.8.4/etc/hadoop目录下hadoop-env.sh、yarn-env.sh的JAVA_HOME，不设置的话，启动不了， export JAVA_HOME=/opt/jdk1.8.0_171


#注意内存分配  yarn-site.xml
<property>  
    <name>yarn.scheduler.maximum-allocation-mb</name>  
    <value>2048</value>  
    <discription>每个节点可用内存,单位MB,默认8182MB</discription>  
</property>  


#复制配置文件到node-2 node-3
scp -r /opt/hadoop-2.8.4/ root@node-2:/opt/
scp -r /opt/hadoop-2.8.4/ root@node-3:/opt/

#初始化
cd /opt/hadoop-2.8.4/bin
./hadoop  namenode  -format
#格式化成功后，可以在看到在/root/hadoop/dfs/name/目录多了一个current目录，而且该目录内有一系列文件

#Wed Jun 20 15:50:27 CST 2018
namespaceID=600364786
clusterID=CID-249602e7-9148-464a-be1d-eea067bce2d6
cTime=1529481027131
storageType=NAME_NODE
blockpoolID=BP-1719617210-192.168.17.132-1529481027131
layoutVersion=-63



#

```

## Hadoop namenode重新格式化需注意问题

```shell
#https://blog.csdn.net/gis_101/article/details/52821946
#1、重新格式化意味着集群的数据会被全部删除，格式化前需考虑数据备份或转移问题； 
#2、先删除主节点（即namenode节点），Hadoop的临时存储目录tmp、namenode存储永久性元数据目录dfs/name、Hadoop系统日志文件目录log 中的内容 （注意是删除目录下的内容不是目录）； 
#3、删除所有数据节点(即datanode节点) ，Hadoop的临时存储目录tmp、namenode存储永久性元数据目录dfs/name、Hadoop系统日志文件目录log 中的内容； 
#4、格式化一个新的分布式文件系统：

hadoop namenode -format  

```



```shell
#配置hadoop的环境变量

export JAVA_HOME=/opt/jdk1.8.0_171
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export HADOOP_HOME=/opt/hadoop-2.8.4
export REDIS_HOME=/opt/redis-3.2.1
export ERL_HOME=/lib/erlang
export PATH=${JAVA_HOME}/bin:${REDIS_HOME}/src:${ERL_HOME}/bin:${HADOOP_HOME}/bin:$PATH

#开启hadoop
cd /opt/hadoop-2.8.4/sbin
./start-all.sh

#测试
http://192.168.17.132:50070
http://192.168.17.132:8088/




```











