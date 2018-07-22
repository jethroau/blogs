
## create registry and customize the storage location. 
```
 docker run -d \
  -p 5000:5000 \
  --restart=always \
  --name registry \
  -v /home/registry:/var/lib/registry \
  registry
```
  
## create a hostnamne
vi /etc/host
127.0.0.1 registry.jethro.com
