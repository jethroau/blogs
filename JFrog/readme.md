# Create Docker register proxy. 

## Create local repository 
New => input repository key => docker_local_hosted  

## Create Remote repository
New => input repository key => docker_official_registry  

## Create Virtual repository
New => input repository key => add docker_local_hosted and docker_official_registry to virtual repository => Save.   

# Set Http setting
Docker access method => repository path  

# Trust http access to docker setting
 sudo vi /etc/docker/daemon.json
 ```
{
  "insecure-registries" : ["<jfrog.domain:8081>"]
}
```
# Test docker pull / push
```docker
docker pull <jfrog.domain>:8081/<REPOSITORY_KEY>/<IMAGE>:<TAG>  
docker login <jfrog.domain>:8081  
docker tag <IMAGE>:<TAG> <jfrog.domain>:8081/<REPOSITORY_KEY>/<IMAGE>:<TAG>  
docker push <jfrog.domain>:8081/<REPOSITORY_KEY>/<IMAGE>:<TAG> 
```
# Enable https
1. Generate Reverse Proxy Settings from Jfrog. 
2. Allow proxy premission in linux. 
```
 /usr/sbin/setsebool -P httpd_can_network_connect 1
```
3. Paste Apache virtual host setting to httpd.conf.  
4. Remove apache orginal SSL <virtual host> stting  
