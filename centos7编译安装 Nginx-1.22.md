### 1、安装编译 Nginx 依赖包
```
# yum -y install gcc gcc-c++ make zlib-devel pcre pcre-devel openssl-devel perl-devel perl-ExtUtils-Embed gd-devel
```
### 2、官网下载 Nginx 安装包
```
# wget https://nginx.org/download/nginx-1.22.1.tar.gz
```
[nginx-1.22.1.tar.gz](https://www.yuque.com/attachments/yuque/0/2023/gz/565263/1685703781023-db3d992e-88d5-4454-b07b-25688766c44c.gz?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2023%2Fgz%2F565263%2F1685703781023-db3d992e-88d5-4454-b07b-25688766c44c.gz%22%2C%22name%22%3A%22nginx-1.22.1.tar.gz%22%2C%22size%22%3A1073948%2C%22ext%22%3A%22gz%22%2C%22source%22%3A%22%22%2C%22status%22%3A%22done%22%2C%22download%22%3Atrue%2C%22taskId%22%3A%22u3387a2d5-8dd9-4e1d-96e8-77907f2afb0%22%2C%22taskType%22%3A%22upload%22%2C%22type%22%3A%22application%2Fx-gzip%22%2C%22__spacing%22%3A%22both%22%2C%22mode%22%3A%22title%22%2C%22id%22%3A%22u3c094d02%22%2C%22margin%22%3A%7B%22top%22%3Atrue%2C%22bottom%22%3Atrue%7D%2C%22card%22%3A%22file%22%7D)
### 3、创建 Nginx 运行用户
```
# useradd -s /sbin/nologin -M nginx
```
### 4、解压配置 Nginx 编译
```
[root@localhost ~ ]# tar zxvf nginx-1.22.1.tar.gz -C /usr/local/
[root@localhost ~]# cd /usr/local/nginx-1.22.1/
[root@localhost nginx-1.22.1]# ./configure \
--user=nginx \
--group=nginx \
--prefix=/usr/local/nginx \
--conf-path=/etc/nginx/nginx.conf \
--sbin-path=/usr/sbin/nginx \
--error-log-path=/var/log/nginx/nginx_error.log \
--http-log-path=/var/log/nginx/nginx_access.log \
--pid-path=/usr/local/nginx/run/nginx.pid
```
### 5、Nginx 编译安装
```
# make && make install
```
### 6、测试 Nginx 是否安装成功
```
[root@localhost nginx-1.22.1]# nginx -V
nginx version: nginx/1.22.1
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) 
configure arguments: --user=nginx --group=nginx --prefix=/usr/local/nginx --conf-path=/etc/nginx/nginx.conf --sbin-path=/usr/sbin/nginx --error-log-path=/var/log/nginx/nginx_error.log --http-log-path=/var/log/nginx/nginx_access.log --pid-path=/usr/local/nginx/run/nginx.pid
```
### 7、启动 Nginx 服务
```
# /usr/sbin/nginx
```
### 8、验证 Nginx 服务是否启动成功
```
# netstat -lntp | grep nginx 
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      29740/nginx: master

# ps aux |grep nginx
```
### 9、系统添加 Nginx 服务
以 systemd 形式添加
##### 1、创建 nginx.service 文件
```
[root@localhost ~]# vim /lib/systemd/system/nginx.service
[Unit]
Description=nginx
After=network.target

[Service]
Type=forking
ExecStart=/usr/sbin/nginx
ExecReload=/usr/sbin/nginx -s reload
ExecStop=/usr/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target


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
##### 2、以 systemctl 方式启动 Nginx
```
[root@localhost ~]# pkill nginx
[root@localhost ~]# systemctl daemon-reload 
[root@localhost ~]# systemctl start nginx
```
##### 3、查看 Nginx 服务状态
```
[root@localhost ~]# ps -ef | grep ngin
root      31469      1  0 23:11 ?        00:00:00 nginx: master process /usr/sbin/nginx
nginx     31470  31469  0 23:11 ?        00:00:00 nginx: worker process
root      31554   1182  0 23:11 pts/0    00:00:00 grep --color=auto ngin
```
##### 4、验证 Nginx 服务是否成功启动
```
[root@localhost ~]# netstat -ntlp | grep nginx
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      349/nginx: master p 
```
##### 5、配置 Nginx 服务自动启动
```
[root@localhost ~]# systemctl enable nginx
Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to /usr/lib/systemd/system/nginx.service.
```
