## 安装Erlang 

版本对照：http://www.rabbitmq.com/which-erlang.html

## 安装erlang依赖项 

```shell
yum -y install make gcc gcc-c++ kernel-devel m4 ncurses-devel unixODBC unixODBC-devel wxGTK SDL wxGTK-gl openssl-devel
```

## 配置erlang的环境变量

```shell
#可以找到安装目录
whereis erl
```



## 安装rabbitmq

```shel
rpm --import https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc
# this example assumes the CentOS 7 version of the package
yum install rabbitmq-server-3.7.6-1.el7.noarch.rpm
```





```shell

#可能会遇到Error when reading /var/lib/rabbitmq/.erlang.cookie: #eacces（这是因为没有权限的问题） 

chown rabbitmq:rabbitmq /var/lib/rabbitmq/.erlang.cookie 



```

## rabbitmq配置文件样板

```shell
cd /usr/share/doc/rabbitmq-server-3.7.5/

cp /usr/share/doc/rabbitmq-server-3.7.5/rabbitmq.config.example /etc/rabbitmq/rabbitmq.config

```

## 外部地址访问

如果想使用guest/guest通过远程机器访问，需要在rabbitmq配置文件中(找到/rabbitmq_server-3.6.14/ebin下面的rabbit.app文件)中设置 

loopback_users为[]。

找到/rabbitmq_server-3.6.14/ebin下面的rabbit.app文件文件完整内容如下（注意后面的半角句号）： 

找到：loopback_users里的<<”guest”>>删除。



```shell
/usr/lib/rabbitmq/lib/rabbitmq_server-3.7.5/ebin
vim rabbit.app
#修改 [{rabbit, [{loopback_users, []}]}]. 然后重启
systemctl restart rabbitmq-server.service 


```







