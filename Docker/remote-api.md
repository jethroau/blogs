## /lib/systemd/system/docker.service
```
ExecStart=/usr/bin/dockerd-current \
          ...
          -H tcp://0.0.0.0:4243 -H unix:///var/run/docker.sock -G xxx_linux_users \
          ...
```
-G, for jenkins or none root to run docker build.   

## verify docker-sysconifg.conf and remove it. 
```
rm /etc/systemd/system/docker.service.d/docker-sysconfig.conf
```

## reload daemon & start docker service
```
sudo systemctl daemon-reload
sudo systemctl restart docker
# check status and see if 4243 argurment has been added to daemon
sudo systemctl status docker
```

## test remote access
```
netstat -ln  | grep 4243
docker version
docker -H localhost:4243 version
curl http://127.0.0.1:4243/version
```


## reference
https://www.cyberciti.biz/faq/install-use-setup-docker-on-rhel7-centos7-linux/  
https://www.cnblogs.com/520playboy/p/7921633.html  
https://blog.csdn.net/lvyuan1234/article/details/69255944
