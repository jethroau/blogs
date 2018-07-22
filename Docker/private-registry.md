# Registry Server (192.168.50.5)
## create registry and customize the storage location. 
```
 docker run -d \
  -p 5000:5000 \
  --restart=always \
  --name registry \
  -v /home/registry:/var/lib/registry \
  registry
```

## volume 
```
docker inspect registry
```
```
...
"Mounts": [
	{
		"Type": "bind",
		"Source": "/home/registry",
		"Destination": "/var/lib/registry",
		"Mode": "",
		"RW": true,
		"Propagation": "rprivate"
	}
],
...
```

# Docker client (192.168.50.104）
## /etc/sysconfig/docker
```
OPTIONS='--selinux-enabled --log-driver=journald --signature-verification=false --add-registry registry.jethro.io:5000'
```
## /etc/docker/daemon.json
```
{
    "registry-mirrors": [
        "http://xxx.m.daocloud.io"
    ],
    "insecure-registries": [
        "192.168.50.5:5000", "registry.jethro.io:5000"
    ]
}

```

## create a hostnamne
```
vi /etc/host
192.168.50.5 registry.jethro.io
```

## docker push / pull
```docker
docker tag hello-world:latest hello:1.0 
docker push hello:1.0
docker rmi hello:1.0

docker pull hello:1.0
```

# Web UI

## list registry image
Listing Repositories 
http://192.168.50.5:5000/v2/_catalog  

## install a Web UI
```
docker pull konradkleine/docker-registry-frontend:v2 
docker run \
  -d \
  -e ENV_DOCKER_REGISTRY_HOST=192.168.50.104 \
  -e ENV_DOCKER_REGISTRY_PORT=5000 \
  -p 8080:80 \  
```
> open browser and enter http://192.168.50.104:8080


## reference 
https://docs.docker.com/registry/deploying/#run-an-externally-accessible-registry
https://blog.csdn.net/mideagroup/article/details/52052618  
https://github.com/kwk/docker-registry-frontend  
https://github.com/mkuchin/docker-registry-web  

