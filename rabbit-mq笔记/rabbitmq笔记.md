安装Erlang 

版本对照：http://www.rabbitmq.com/which-erlang.html

安装erlang依赖项 

```shell
yum -y install make gcc gcc-c++ kernel-devel m4 ncurses-devel unixODBC unixODBC-devel wxGTK SDL wxGTK-gl openssl-devel
```

配置erlang的环境变量

```shell
#可以找到安装目录
whereis erl
```



安装rabbitmq

```shel
rpm --import https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc
# this example assumes the CentOS 7 version of the package
yum install rabbitmq-server-3.7.6-1.el7.noarch.rpm
```





```shell

#可能会遇到Error when reading /var/lib/rabbitmq/.erlang.cookie: #eacces（这是因为没有权限的问题） 

chown rabbitmq:rabbitmq /var/lib/rabbitmq/.erlang.cookie 



```





