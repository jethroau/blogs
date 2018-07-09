## Command
```docker
docker pull registry:tag  
docker image ls/rm  
docker image ls -f dangling=true
docker image prune
docker run -it registry:tag bash
```
> **-f dangling=true**， list all <none> images  
> **prune**， remove all <none> images  
> **-i**， interactive  
> **-t**， terminal  
> **bash**， first execute command when container starts
