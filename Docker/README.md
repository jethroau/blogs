## image 
```docker
docker pull name:tag  
docker image ls/rm  
docker image ls -f dangling=true
docker image prune
docker image rm <name:tag>/<IMAGE ID>
docker rmi [OPTIONS] IMAGE [IMAGE...]
docker images -a | grep "pattern" | awk '{print $3}' | xargs docker rmi
```
> **pull**, download image to local 
> **-f dangling=true**， list all <none> images  
> **prune**， remove all <none> images  
> **-i**， interactive  
> rmi = image rm 

## container 
```Docker
docker run -it name:tag bash
docker run --name webserver -d -p 80:80 nginx
docker exec -it webserver/<container id> bash
docker container rm <container id>
docker logs <container id>
docker rm [OPTIONS] CONTAINER [CONTAINER...]
docker rm $(docker ps -a -f status=exited -q)
docker ps -a
docker ps | grep "vmware" | awk '{print $1}' | xargs docker stop
```
> **-t**， terminal  
> **bash**， first execute command when container starts  
> **name**, container name  
> **-p**, enable outside port from docker   
> **-d**, run as background job
> **exec**, enter terminal mode and its exit will not stop container   
> **-a**, show all container (default shows running container)

## Clean all image, container and volume
```
docker ps | awk '{print $1}' | xargs docker stop
docker rm -f $(docker ps -qa)
docker rmi -f $(docker images -q)
docker volume rm $(docker volume ls -q)
```

## build
```Docker
docker build -t name:tag . 
```
> **-t** specify new image name and tag  
> **.** context to build image 

## system
```Docker
docker system prune
docker system df
```

## enter docker 
```
yum install util-linux  
------------
#docker_enter.sh
#/bin/bash
pid=`docker inspect --format "{{.State.Pid}}" $1`
nsenter --target "$pid" --mount --uts --ipc --net --pid  /bin/su - root

```
> docker ps  
> docker_enter.sh <container_id>

[Docker容器进入的4种方式](https://www.cnblogs.com/xhyan/p/6593075.html)


## Dockerfile
```Docker
FROM name:tag
ADD / COPY
WORKDIR
RUN / CMD / ENTRYPOINT  <exec>/<shell>
```
> **ADD**, can auto-uncompress tar/gz/zip files  
> **WORKDIR**, switch current working path  
> **ENTRYPOINT**, can include argruments from outside  

## Reference
[How To Remove Docker Images, Containers, and Volumes](https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes)  
[如何清理Docker占用的磁盘空间](http://dockone.io/article/3056)
