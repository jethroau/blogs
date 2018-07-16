## /lib/systemd/system/docker.service
```
ExecStart=/usr/bin/dockerd-current \
          ...
          -H tcp://0.0.0.0:4243 -H unix:///var/run/docker.sock \
          ...
```

## reload daemon & start docker service
```
systemctl daemon-reload
systemctl restart docker
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
