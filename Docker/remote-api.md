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
systemctl start docker
```


## reference
https://www.cyberciti.biz/faq/install-use-setup-docker-on-rhel7-centos7-linux/  
https://www.cnblogs.com/520playboy/p/7921633.html  
