# MySql5.7.22安装

```shell
mkdir -p /opt/mysql57/data
#解压 
tar -zxvf mysql-5.7.22-el7-x86_64.tar.gz
#复制     
cp -r mysql-5.7.22-el7-x86_64/. /opt/mysql57



# 添加系统mysql组   
groupadd mysql

# 添加mysql用户,添加完成后可用id mysql查看   
useradd -r -g mysql mysql  
#切到mysql目录
cd /opt/mysql57
#修改当前目录拥有者为mysql用户      
chown -R mysql:mysql ./

#安装数据库      
bin/mysqld --initialize --user=mysql --basedir=/opt/mysql57 --datadir=/opt/mysql57/data

#生成了临时密码 如下图

#执行以下命令创建RSA private key 
bin/mysql_ssl_rsa_setup  --datadir=/opt/mysql57/data

#修改当前目录拥有者为mysql用户  
chown -R mysql:mysql ./ 

#修改当前data目录拥有者为mysql用户  
chown -R mysql:mysql data

#配置my.cnf  
vim /etc/my.cnf    

[mysqld]  
character_set_server=utf8  
init_connect='SET NAMES utf8'  
basedir=/opt/mysql57/
datadir=/opt/mysql57/data  
socket=/tmp/mysql.sock  
#不区分大小写  
lower_case_table_names = 1  
log-error=/var/log/mysqld.log  
pid-file=/opt/mysql57/data/mysqld.pid  


#添加开机启动
cp /opt/mysql57/support-files/mysql.server  /etc/init.d/mysqld
  
vim /etc/init.d/mysqld   
#添加路径
basedir=/usr/local/mysql  
datadir=/usr/local/mysql/data  


service mysqld start
#添加软连接
ln -s /opt/mysql57/bin/mysql /usr/bin
mysql -uroot -p 
#输入上面生成的随机密码
#第一件事先修改密码
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('root');
```



![生成随机密码](.\img\生成随机密码.jpg)