关闭防火墙和selinux
### 编译安装LNMP
#### 编译安装Mysql
##### 1.1 清理安装环境
```
# yum erase mariadb mariadb-server mariadb-libs mariadb-devel -y
# userdel -r mysql
# rm -rf /etc/my*
# rm -rf /var/lib/mysql
```
##### 1.2 创建mysql用户
```
[root@localhost ~]# useradd -r mysql -M -s /bin/nologin
-M 不创建用户的家目录
```
##### 1.3、从官网下载tar包
```
wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-boost-5.7.27.tar.gz
```
[mysql-boost-5.7.27.tar.gz](https://www.yuque.com/attachments/yuque/0/2023/gz/565263/1685703528555-610a1110-bedf-475b-9314-a68d742c29be.gz?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2023%2Fgz%2F565263%2F1685703528555-610a1110-bedf-475b-9314-a68d742c29be.gz%22%2C%22name%22%3A%22mysql-boost-5.7.27.tar.gz%22%2C%22size%22%3A51436383%2C%22ext%22%3A%22gz%22%2C%22source%22%3A%22%22%2C%22status%22%3A%22done%22%2C%22download%22%3Atrue%2C%22taskId%22%3A%22u9042dc28-dd4b-44e1-918e-9fd02ba03ae%22%2C%22taskType%22%3A%22upload%22%2C%22type%22%3A%22application%2Fx-gzip%22%2C%22__spacing%22%3A%22both%22%2C%22id%22%3A%22u63dbe417%22%2C%22margin%22%3A%7B%22top%22%3Atrue%2C%22bottom%22%3Atrue%7D%2C%22card%22%3A%22file%22%7D)
##### 1.4、安装编译工具
```
# yum -y install ncurses ncurses-devel openssl-devel bison gcc gcc-c++ make glibc automake autoconf
cmake:
# yum -y install cmake
```
##### 1.5、解压
```
[root@localhost ~]# tar xzvf mysql-boost-5.7.27.tar.gz -C /usr/local/
```
##### 1.6 编译安装
```
[root@localhost ~]# cd /usr/local/mysql-5.7.27/
[root@localhost mysql-5.7.27]# cmake . \
-DWITH_BOOST=boost/boost_1_59_0/ \
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DSYSCONFDIR=/etc \
-DMYSQL_DATADIR=/usr/local/mysql/data \
-DINSTALL_MANDIR=/usr/share/man \
-DMYSQL_TCP_PORT=3306 \
-DMYSQL_UNIX_ADDR=/tmp/mysql.sock \
-DDEFAULT_CHARSET=utf8 \
-DEXTRA_CHARSETS=all \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_READLINE=1 \
-DWITH_SSL=system \
-DWITH_EMBEDDED_SERVER=1 \
-DENABLED_LOCAL_INFILE=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1

参数详解:
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \   安装目录
-DSYSCONFDIR=/etc \   配置文件存放 （默认可以不安装配置文件）
-DMYSQL_DATADIR=/usr/local/mysql/data \   数据目录   错误日志文件也会在这个目录
-DINSTALL_MANDIR=/usr/share/man \     帮助文档 
-DMYSQL_TCP_PORT=3306 \     默认端口
-DMYSQL_UNIX_ADDR=/tmp/mysql.sock \  sock文件位置，用来做网络通信的，客户端连接服务器的时候用
-DDEFAULT_CHARSET=utf8 \    默认字符集。字符集的支持，可以调
-DEXTRA_CHARSETS=all \   扩展的字符集支持所有的
-DDEFAULT_COLLATION=utf8_general_ci \  支持的
-DWITH_READLINE=1 \    上下翻历史命令
-DWITH_SSL=system \    使用私钥和证书登陆（公钥）  可以加密。 适用与长连接。坏处：速度慢
-DWITH_EMBEDDED_SERVER=1 \   嵌入式数据库
-DENABLED_LOCAL_INFILE=1 \    从本地倒入数据，不是备份和恢复。
-DWITH_INNOBASE_STORAGE_ENGINE=1  默认的存储引擎，支持外键
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/565263/1685674811034-ca297e9e-f275-424d-a500-213bcecfad2f.png#averageHue=%23090807&clientId=udcc0a0c9-f85c-4&from=paste&height=209&id=ub1881102&originHeight=230&originWidth=955&originalType=binary&ratio=1&rotation=0&showTitle=false&size=39319&status=done&style=none&taskId=uc91f2c47-5d40-4b01-84bb-63a694e923a&title=&width=868.1817993644845)
```
[root@localhost mysql-5.7.27]# make && make install
```
##### 1.7 初始化
```
[root@localhost mysql-5.7.27]# cd /usr/local/mysql
[root@localhost mysql]# chown -R mysql.mysql .
[root@localhost mysql]# ./bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data     ---初始化完成之后，一定要记住提示最后的密码用于登陆或者修改密码
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/565263/1685674800332-0ba0308c-db45-4985-ab2a-60069c80cd8d.png#averageHue=%2314100d&clientId=udcc0a0c9-f85c-4&from=paste&height=268&id=ua2103d9f&originHeight=295&originWidth=991&originalType=binary&ratio=1&rotation=0&showTitle=false&size=216759&status=done&style=none&taskId=u23f6e5b4-7e84-4af7-9b0d-12c804fc802&title=&width=900.9090713824127)
 初始化,只需要初始化一次
```
设置环境变量
[root@localhost mysql]# echo 'export PATH=/usr/local/mysql/bin:$PATH' >>/etc/profile
[root@localhost mysql]# source /etc/profile
[root@localhost mysql]# echo $PATH
/usr/local/mysql/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin

[root@localhost ~]# vim /etc/my.cnf  --如果打开文件有内容将文件中所有内容注释掉，在添加如下内容
[client]
port = 3306
socket = /tmp/mysql.sock
default-character-set = utf8

[mysqld]
port = 3306
user = mysql
basedir = /usr/local/mysql  #指定安装目录
datadir = /usr/local/mysql/data  #指定数据存放目录
socket = /tmp/mysql.sock
character_set_server = utf8



[client]
# 默认连接端口
port = 3306
# 用于本地连接的socket套接字
socket = /tmp/mysql.sock
# 编码
default-character-set = utf8

[mysqld]
# 服务端口号，默认3306
port = 3306
# mysql启动用户
user = mysql
# mysql安装根目录
basedir = /usr/local/mysql
# mysql数据文件所在位置
datadir = /usr/local/mysql/data
# 为MySQL客户端程序和服务器之间的本地通讯指定一个套接字文件
socket = /tmp/mysql.sock
# 数据库默认字符集,主流字符集支持一些特殊表情符号(特殊表情符占用4个字节)
character_set_server = utf8
```
##### 1.8 启动mysl
```
[root@localhost ~]# cd /usr/local/mysql
[root@localhost mysql]# ./bin/mysqld_safe --user=mysql &

启动之后再按一下回车！即可后台运行
```
##### 1.9 登录mysl
```
[root@localhost mysql]# /usr/local/mysql/bin/mysql -uroot -p'GP9TKGgY9i/8'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.27

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
```
##### 1.10 systemctl启动方式
拷贝启动脚本到/etc/init.d/目录下，并改名mysqld
```
[root@localhost mysql]# cp support-files/mysql.server /etc/init.d/mysqld
[root@localhost mysql]# ls -l /etc/init.d/mysqld
-rwxr-xr-x 1 root root 10588 Aug 1 18:33 /etc/init.d/mysqld
```
重新加载系统服务
```
[root@localhost mysql]# systemctl daemon-reload
```
启动MySQL数据库，并检查端口监听状态
```
[root@localhost mysql]# systemctl stop mysqld   --停止mysqld
# 或者
[root@localhost mysql]# systemctl start mysqld  --启动mysqld
Starting MySQL. SUCCESS! 

[root@localhost mysql]# netstat -lntp | grep 3306
tcp6       0      0 :::3306                 :::*                    LISTEN      16744/mysqld 
```
##### 1.11 数据库密码修改
```
[root@localhost ~]# /usr/local/mysql/bin/mysqladmin -uroot -p'GP9TKGgY9i/8' password '要修改的密码'
[root@localhost ~]# /usr/local/mysql/bin/mysql -uroot -p'新密码'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5
Server version: 5.7.27

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> create database wordpress;
mysql> grant all on *.* to 'remote'@'%' identified by '123';
mysql> flush privileges;
```
