## image 
```docker
docker pull name:tag  
docker image ls/rm  
docker image ls -f dangling=true
docker image prune
docker image rm <name:tag>/<IMAGE ID>

```
> **-f dangling=true**， list all <none> images  
> **prune**， remove all <none> images  
> **-i**， interactive  

## container 
```Docker
docker run -it name:tag bash
docker run --name webserver -d -p 80:80 nginx
docker exec -it webserver bash
```
> **-t**， terminal  
> **bash**， first execute command when container starts  
> **name**, container name  
> **-p**, enable outside port from docker   
> **-d**, run as background job

## build
```Docker
docker build -t name:tag . 
```
> **-t** specify new image name and tag  
> **.** contenxt to build image 
