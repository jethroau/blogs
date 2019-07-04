## installation
1. download offline instlaler  
https://github.com/goharbor/harbor/releases   

2. install docker and docker-compose
```
# install dockder
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
  
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo  
    # 国内使用aliyun加速 http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    
yum install docker-ce

# 国内使用加速器, get your personal accelerator via https://www.daocloud.io/mirror
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io

systemctl daemon-reload  && systemctl restart docker

# install docker-compose
wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-1.4.2.tar.gz
tar -vxf setuptools-1.4.2.tar.gz
cd setuptools-1.4.2
python2.7 setup.py install		//因为服务器自带 Python 2.7
easy_install-2.7 pip

pip install docker-compose
docker-compose --version	//测试安装是否成功
```

3. unzip and config
```
tar zxvf harbor-offline-installer-vxxx.tgz
cd harbor
vi harbor.cfg
<<EOF
  #set hostname
  hostname = yourdomain.com:port
  #set ui_url_protocol
  ui_url_protocol = https
  ......
  #The path of cert and key files for nginx, they are applied only the protocol is set to https 
  ssl_cert = /data/cert/yourdomain.com.crt
  ssl_cert_key = /data/cert/yourdomain.com.key
EOF
```

4. enable TLS for harbor  
[Generate SSL cert](https://github.com/jethroau/blogs/blob/master/Docker/tls-auth-registry.md#server-side)  
[Configuring Harbor with HTTPS Access](https://github.com/goharbor/harbor/blob/master/docs/configure_https.md)

5. run install after config done
```
./install
```

6. change config
```
cd harbor
vi harbor.cfg
./prepare 
docker-compose down -v
docker-compose up -d
```

7. config/test docker client connection  
[install cert for docker client](https://github.com/jethroau/blogs/blob/master/Docker/tls-auth-registry.md#install-cert-for-docker-client)  
```
docker login  yourdomain.com:port

docker tag hello-world:latest hello:1.0 
docker push hello:1.0
docker rmi hello:1.0

docker pull hello:1.0
```


## Reference
[基于 Harbor 搭建 Docker 私有镜像仓库](https://zhuanlan.zhihu.com/p/31483386)  
[搭建Harbor企业级docker仓库](https://www.cnblogs.com/pangguoping/p/7650014.html)
[Openshift 和Harbor的集成]　https://www.cnblogs.com/ericnie/p/10099856.html


## Github
https://github.com/goharbor/harbor  
