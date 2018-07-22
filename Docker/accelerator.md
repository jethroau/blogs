
## 163.com
http://hub-mirror.c.163.com/

## daocloud.io
https://www.daocloud.io/mirror#accelerator-doc
github account login

## centos
```
vi  /etc/docker/daemon.json
{
    "registry-mirrors": [
        "http://xxx.xxx"
    ],
    "insecure-registries": []
}
```

## toolbox
http://8caa65c6.m.daocloud.io
```
docker-machine ssh default
sudo sed -i "s|EXTRA_ARGS='|EXTRA_ARGS='--registry-mirror=http://xxx.xxx |g" /var/lib/boot2docker/profile
exit
docker-machine restart default 
```

