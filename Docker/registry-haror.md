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

# install docker-compose
wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-1.4.2.tar.gz
tar -vxf setuptools-1.4.2.tar.gz
cd setuptools-1.4.2
python2.7 setup.py install		//因为服务器自带 Python 2.7
easy_install-2.7 pip

pip install docker-compose
docker-compose --version	//测试安装是否成功
```

3. 


## Reference
[基于 Harbor 搭建 Docker 私有镜像仓库](https://zhuanlan.zhihu.com/p/31483386)

## Github
https://github.com/goharbor/harbor  
