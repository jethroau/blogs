# Create Docker register proxy. 

## Create local repository 
New => input repository key => docker_local_hosted  

## Create Remote repository
New => input repository key => docker_official_registry  

## Create Virtual repository
New => input repository key => add docker_local_hosted and docker_official_registry to virtual repository => Save.   

# Set Http setting. 
Docker access method => repository path  

# docker setting. 
 sudo vi /etc/docker/daemon.json
 ```
{
  "insecure-registries" : ["<jfrog.domain:8081>"]
}
```
# try docker pull / push
```docker
docker pull <jfrog.domain>:8081/<REPOSITORY_KEY>/<IMAGE>:<TAG>  
docker login <jfrog.domain>:8081  
docker tag <IMAGE>:<TAG> <jfrog.domain>:8081/<REPOSITORY_KEY>/<IMAGE>:<TAG>  
docker push <jfrog.domain>:8081/<REPOSITORY_KEY>/<IMAGE>:<TAG> 
```
 
