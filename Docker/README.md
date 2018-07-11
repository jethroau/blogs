## image 
```docker
docker pull registry:tag  
docker image ls/rm  
docker image ls -f dangling=true
docker image prune
docker image rm <registry:tag>/<IMAGE ID>

```
> **-f dangling=true**， list all <none> images  
> **prune**， remove all <none> images  
> **-i**， interactive  

## container 
```Docker
docker run -it registry:tag bash
docker run --name webserver -d -p 80:80 nginx
docker exec -it webserver bash
```
> **-t**， terminal  
> **bash**， first execute command when container starts  
> **name**, container name  
> **-p**, enable outside port from docker   
> **-d**, run as background job
