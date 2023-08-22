### 修改网卡配置文件

```ini
[root@localhost ~]# vim /etc/sysconfig/network-scripts/ifcfg-ens33
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
#将dhcp修改为static
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="0ed8193f-42e1-4379-bb46-570ee8f89e7e"
DEVICE="ens33"
#系统启动时是否激活网卡
ONBOOT="yes"
#以下新增以实际为准
#设置静态IP地址
IPADDR=10.12.154.28
#设置子网掩码
NETMASK=255.255.255.0
#设置网关
GATEWAY=10.12.154.254
#设置DNS
DNS1=10.12.154.254

重启网络服务
[root@localhost ~]# systemctl restart network
```