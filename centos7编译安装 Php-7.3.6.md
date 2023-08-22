#### 1、下载 Php 源码包
```
# wget https://www.php.net/distributions/php-7.3.6.tar.gz
# yum install -y apr* autoconf automake bison bzip2 bzip2* cloog-ppl cpp curl curl-devel fontconfig fontconfig-devel freetype freetype* freetype-devel gcc gcc-c++ gtk+-devel gd gettext gettext-devel glibc kernel kernel-headers keyutils keyutils-libs-devel krb5-devel libcom_err-devel libpng libpng-devel libjpeg* libsepol-devel libselinux-devel libstdc++-devel libtool* libgomp libxml2 libxml2-devel libXpm* libxml* libXaw-devel libXmu-devel libtiff libtiff* make mpfr ncurses* ntp openssl openssl-devel patch pcre-devel perl php-common php-gd policycoreutils telnet t1lib t1lib* nasm nasm* wget zlib-devel
```
[php-7.3.6.tar.gz](https://www.yuque.com/attachments/yuque/0/2023/gz/565263/1685703828201-d350190f-7344-4515-a243-c57349db135e.gz?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2023%2Fgz%2F565263%2F1685703828201-d350190f-7344-4515-a243-c57349db135e.gz%22%2C%22name%22%3A%22php-7.3.6.tar.gz%22%2C%22size%22%3A19449322%2C%22ext%22%3A%22gz%22%2C%22source%22%3A%22%22%2C%22status%22%3A%22done%22%2C%22download%22%3Atrue%2C%22taskId%22%3A%22u7b0dcde4-8742-46a1-9eec-40ffcd71025%22%2C%22taskType%22%3A%22upload%22%2C%22type%22%3A%22application%2Fx-gzip%22%2C%22__spacing%22%3A%22both%22%2C%22mode%22%3A%22title%22%2C%22id%22%3A%22u5c1ef744%22%2C%22margin%22%3A%7B%22top%22%3Atrue%2C%22bottom%22%3Atrue%7D%2C%22card%22%3A%22file%22%7D)
#### 2、配置 Php 编译
```
[root@localhost ~]# yum install libxml2-devel curl-devel
[root@localhost ~]# tar xzvf php-7.3.6.tar.gz -C /usr/local/
[root@localhost ~]# cd /usr/local/php-7.3.6/
[root@localhost php-7.3.6 ]# ./configure \
    --prefix=/usr/local/php7 \
    --with-config-file-path=/usr/local/php7 \
    --with-config-file-scan-dir=/usr/local/php7/php.d \
    --enable-mysqlnd \
    --with-mysqli \
    --with-pdo-mysql \
    --enable-fpm \
    --with-fpm-user=nginx \
    --with-fpm-group=nginx \
    --with-gd \
    --with-iconv \
    --enable-xml \
    --enable-shmop \
    --enable-sysvsem \
    --enable-inline-optimization \
    --enable-mbregex \
    --enable-mbstring \
    --enable-ftp \
    --with-openssl \
    --enable-pcntl \
    --enable-sockets \
    --with-xmlrpc \
    --enable-soap \
    --without-pear \
    --with-gettext \
    --enable-session \
    --with-curl \
    --with-jpeg-dir \
    --with-freetype-dir \
    --enable-opcache
```
#### 3、Php 编译参数说明
```
--prefix=/usr/local/php7 		# 配置安装目录
--with-config-file-path=/usr/local/php7 # 配置文件 php.ini 的路径
--enable-sockets 				# 开启 socket 
--enable-fpm 					# 启用 fpm 扩展
--enable-cli			 		# 启用 命令行模式 (从 php 4.3.0 之后这个模块默认开启所以可以不用再加此命令)
--enable-mbstring 				# 启用 mbstring 库
--enable-pcntl 					# 启用 pcntl (仅 CLI / CGI)
--enable-soap 					# 启用 soap 
--enable-opcache 				# 开启 opcache 缓存
--disable-fileinfo 				# 禁用 fileinfo (由于 5.3+ 之后已经不再持续维护了，但默认是开启的，所以还是禁止了吧)(1G以下内存服务器直接关了吧)
--disable-rpath  				# 禁用在搜索路径中传递其他运行库。
--with-mysqli 					# 启用 mysqli 扩展
--with-pdo-mysql 				# 启用 pdo 扩展
--with-iconv-dir 				# 启用 XMLRPC-EPI 字符编码转换 扩展
--with-openssl 					# 启用 openssl 扩展 (需要 openssl openssl-devel)
--with-fpm-user=nginx 			# 设定 fpm 所属的用户 
--with-fpm-group=nginx 			# 设定 fpm 所属的组别
--with-curl 					# 启用 curl 扩展
--with-mhash 					# 开启 mhash 基于离散数学原理的不可逆向的php加密方式扩展库
# GD
--with-gd 						# 启用 GD 图片操作 扩展
--with-jpeg-dir 				# 开启对 jpeg 图片的支持 (需要 libjpeg)
--with-png-dir 					# 开启对 png 图片支持 (需要 libpng)
--with-freetype-dir 			# 开启 freetype 

# xml
--enable-simplexml 				# 启用对 simplexml 支持
--with-libxml-dir 				# 启用对 libxml2 支持

--enable-debug 					# 开启 debug 模式
```
#### 4、编译安装 Php
```
[root@localhost php-7.3.6]# make && make install 
```
![](https://img.beyourself.org.cn/4199fb8ad000b39339758b614920fc902767a464.jpg#id=YvBH6&originHeight=341&originWidth=1029&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
大约需要等待30分钟
#### 5、创建 php.ini 配置文件
```
[root@localhost php-7.3.6]# cp php.ini-production /usr/local/php7/etc/php.ini

[root@localhost php-7.3.6]# vim /usr/local/php7/etc/php.ini 
#1371注释
session.save_path = "/tmp" 
```
#### 6、设置php-fpm配置文件
```
[root@localhost php-7.3.6]# cd /usr/local/php7/etc
[root@localhost etc]# cp php-fpm.conf.default php-fpm.conf
[root@localhost etc]# vim php-fpm.conf 
#17行注释
修改：pid = /var/run/php-fpm.pid

# php-fpm连接文件
[root@localhost etc]# cd /usr/local/php7/etc/php-fpm.d/
[root@localhost php-fpm.d]# cp www.conf.default www.conf
[root@localhost php-fpm.d]# vim www.conf
user = nginx
group = nginx
listen = 127.0.0.1:9000
```
#### 7、启动 php-fpm
```
[root@localhost php-fpm.d]# /usr/local/php7/sbin/php-fpm
```
#### 8、检查 php-fpm 是否成功启动
```
[root@localhost php-fpm.d]# ps aux | grep php-fpm
```
若看到相关进程，则证明启动成功。查询进程时，进程是以 nginx 用户身份执行的
#### 9、配置 php-fpm 系统环境变量
```
[root@localhost php-fpm.d]# cd
[root@localhost ~]# vim /etc/profile
export PHP_HOME=/usr/local/php7
export PATH=$PATH:$PHP_HOME/bin:$PHP_HOME/sbin
```
#### 10、重载环境变量
```
[root@localhost ~]# source /etc/profile
```

- 使用 echo $PATH 命令查看环境变量中是否已经加入了相关的路径
#### 11、配置 php-fpm 开机自启动
```
[root@localhost ~]# vim /lib/systemd/system/php-fpm.service
[Unit]
Description=php-fpm
After=network.target
[Service]
Type=forking
ExecStart=/usr/local/php7/sbin/php-fpm
ExecStop=/bin/pkill -9 php-fpm
PrivateTmp=true
[Install]
WantedBy=multi-user.target
```
#### 12、php-fpm.service 文件说明
```
[Unit]:服务的说明
Description:描述服务
After:描述服务类别
[Service]服务运行参数的设置
Type=forking是后台运行的形式
ExecStart为服务的具体运行命令
ExecReload为重启命令
ExecStop为停止命令
PrivateTmp=True表示给服务分配独立的临时空间
注意：[Service]的启动、重启、停止命令全部要求使用绝对路径
[Install]运行级别下服务安装的相关设置，可设置为多用户，即系统运行级别为3
```
#### 13、重载 systemctl 配置
```
[root@localhost ~]# systemctl daemon-reload
```
#### 14、停止 php-fpm 
```
[root@localhost ~]# pkill php-fpm
```
#### 15、用 systemctl 启动 php-fpm
```
[root@localhost ~]# systemctl start php-fpm.service
```
#### 16、设置 php-fpm 开机启动
```
[root@localhost ~]# systemctl enable php-fpm.service
```
#### 17、php-fpm 管理命令
```
[root@localhost ~]# systemctl stop php-fpm.service 			# 停止服务
[root@localhost ~]# systemctl restart php-fpm.service 		# 重新启动服务
[root@localhost ~]# systemctl status php-fpm.service		# 查看服务当前状态
[root@localhost ~]# systemctl disable php-fpm.service 		# 停止开机自启动
```
