
# Server Side 

### Generate CA and private key & website TLS certificate 
```bash
cd /etc/docker
mkdir ssl
cd ssl

openssl genrsa -out "root-ca.key" 4096
  
openssl req \
-new -key "root-ca.key" \
-out "root-ca.csr" -sha256 \
-subj '/C=CN/ST=GD/L=GZ/O=JETHRO/CN=Docker Registry CA'

#root-ca.cnf
[root_ca]
basicConstraints = critical,CA:TRUE,pathlen:1
keyUsage = critical, nonRepudiation, cRLSign, keyCertSign
subjectKeyIdentifier=hash

openssl x509 -req -days 3650 -in "root-ca.csr" \
-signkey "root-ca.key" -sha256 -out "root-ca.crt" \
-extfile "root-ca.cnf" -extensions root_ca

openssl genrsa -out "registry.jethro.io.key" 4096

openssl req -new -key "registry.jethro.io.key" -out "registry.csr" -sha256 \
-subj '/C=CN/ST=GD/L=GZ/O=JETHRO/CN=registry.jethro.io'

#registry.cnf
[server]
authorityKeyIdentifier=keyid,issuer
basicConstraints = critical,CA:FALSE
extendedKeyUsage=serverAuth
keyUsage = critical, digitalSignature, keyEncipherment
subjectAltName = DNS:registry.jethro.io, IP:192.168.50.5
subjectKeyIdentifier=hash

openssl x509 -req -days 75000 -in "registry.csr" -sha256 \
-CA "root-ca.crt" -CAkey "root-ca.key" -CAcreateserial \
-out "registry.jethro.io.crt" -extfile "registry.cnf" -extensions server
```

### start docker container registry with TLS
```docker
docker run -d \
  --restart=always \
  --name registry \
  -v /etc/docker/ssl/certs:/certs \
  -v /home/tls-registry:/var/lib/registry  \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/registry.jethro.io.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/registry.jethro.io.key \
  -p 443:443 \
  registry:2
```

# Client side 

### scp CA crt to /etc/docker/cert.d
```bash
cd /etc/docker/certs.d/
mkdir registry.jethro.io
cd registry.jethro.io
scp root@192.168.x.x:/etc/docker/ssl/root-ca.crt ./ca.crt 
#or scp root@192.168.x.x:/etc/docker/ssl/domain-xxx.crt ./client.crt
```

### test docker push & pull
```
docker tag spp-hello:1.0 registry.jethro.io/spp-hello
docker push registry.jethro.io/spp-hello
docker image rm registry.jethro.io/spp-hello
docker pull registry.jethro.io/spp-hello
```

# Restricting access at Server side
### Basic auth

``` 
cd /etc/docker
mkdir auth

docker run \
  --entrypoint htpasswd \
  --name gen-htpasswd
  registry:2 -Bbn jethro jethro > auth/htpasswd
  
docker container rm gen-htpasswd
```

### start registry with TLS and basic auth
```
docker run -d \
  --restart=always \
  --name tls-auth-registry  \
  -v /home/tls-auth-registry:/var/lib/registry  \
  -v /etc/docker/auth:/auth \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=registry-realm" \
  -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
  -v /etc/docker/ssl:/certs \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/registry.jethro.io.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/registry.jethro.io.key \
  -p 443:443 \
  registry:2
```

### Open URL in browser
https://registry.jethro.io/v2/_catalog  

### Login in regitry via a docker client
```
docker login registry.jethro.io
```
### install cert for docker client
Copy the registry.jethro.crt file to /etc/docker/certs.d/registry.jethro.crt/ca.crt on every Docker host.  
You do not need to restart Docker.

## delete registry
https://blog.csdn.net/l6807718/article/details/52886546  
https://github.com/burnettk/delete-docker-registry-image  

# release registry space

## stop register cr.jethro.io
docker ps #copy register:2 container-id and stop it.  
docker stop <container-id>   

## remove repostiroy
rm -rf /docker/tls-auth-registry/docker/registry   

## start register
docker start <container-id>   

## verify register
set <<HOST>> for local PC   
192.168.x.x cr.jethro.io   

https://cr.jethro.io/v2/_catalog   

if above not work, please remove container and start again.   
## stop and remove register.
docker stop <container-id>   
docker rm <container-id>   

## start
```
docker run -d \
  --restart=always \
  --name tls-auth-registry  \
  -v /home/tls-auth-registry:/var/lib/registry  \
  -v /docker/auth:/auth \
  -v /docker/certs:/certs \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=registry-realm" \
  -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/cr.jethro.io.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/cr.jethro.io.key \
  -p 443:443 \
  registry:2
```



# Reference
https://docs.docker.com/registry/deploying/#support-for-lets-encrypt  
https://docs.docker.com/registry/insecure/  
