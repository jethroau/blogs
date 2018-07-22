
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

##
  
## create a hostnamne
```
vi /etc/host
127.0.0.1 registry.jethro.com
```

## list registry image
Listing Repositories 
http://192.168.50.5:5000/v2/_catalog  





## reference 
https://docs.docker.com/registry/deploying/#run-an-externally-accessible-registry
https://blog.csdn.net/mideagroup/article/details/52052618

