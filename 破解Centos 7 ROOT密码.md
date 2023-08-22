```
修复模式：
1，特权模式：不需要root密码，直接以root账户身份登陆。
破解密码时特权模式。
```
```
1.重起系统,进入grub菜单
2.选择要使用的内核
3.按e
```
![](https://img.beyourself.org.cn/b884c9bff6a953a3c30a1367ea2d7c634d9cf897.jpg#id=rZTwF&originHeight=457&originWidth=814&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
```
4.找到linux16那一行，把光标移动到最后，添加 init=/bin/sh
5.ctrl+x #保存退出
```
![](https://img.beyourself.org.cn/74f17959645c15358f80d01a7689ede3b6bef9a7.jpg#id=vFaOn&originHeight=376&originWidth=841&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
```
6.进入系统后，以rw方式重新挂载/分区
#mount -o remount,rw   /
7.永久关闭selinux
#vim /etc/sysconfig/selinux
8.修改密码
passwd 
```
![](https://img.beyourself.org.cn/f420d31ebabdc8bd7b07f38cd5d87a3f74259206.jpg#id=MDx2v&originHeight=102&originWidth=458&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
```
9.  # touch /.autorelabel  #重新识别新的root密码
10. # exec /sbin/init  #重启机器，
```
