
## change time zone without reboot machines. 
```
[root@localhost ~]# cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
cp: overwrite `/etc/localtime'? y

[root@localhost ~]# date
Sat Feb 20 16:04:43 CST 2010
 
[root@localhost ~]# hwclock
```

> https://blog.csdn.net/eagle1830/article/details/62042917  


## sync date time vi NTP
```
yum install ntpdate
date
ntpdate cn.pool.ntp.org
date
```

> https://blog.csdn.net/achang21/article/details/46693545
