## install minikube in centOS7. 
1.install kubectl
```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
       http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
yum install -y kubectl
```
2.install minkube
```bash
curl -Lo minikube http://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/releases/v0.24.1/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```
3. install VirtualBox
```bash
cat <<EOF > /etc/yum.repos.d/virtualbox.repo
[virtualbox]
name=Oracle Linux / RHEL / CentOS-$releasever / $basearch - VirtualBox
baseurl=http://download.virtualbox.org/virtualbox/rpm/el/$releasever/$basearch
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://www.virtualbox.org/download/oracle_vbox.asc
EOF
yum install VirtualBox-5.2
```
3. start minkube
```bash
minikube start --registry-mirror=https://registry.docker-cn.com
```

[Kubernetes 学习笔记之 MiniKube 安装](https://ehlxr.me/2018/01/12/kubernetes-minikube-installation/)  
[Minikube体验](http://www.cnblogs.com/cocowool/p/minikube_setup_and_first_sample.html)  
[kubernetes安装](https://blog.csdn.net/chang_li/article/details/81185631)  
[CentOS 7.2 配置Golang 1.11开发环境](https://yq.aliyun.com/articles/645569)


## Reference
[kubenetes Tutorials](https://kubernetes.io/docs/tutorials/)  
[简单的 Kubernetes 集群搭建](https://soulteary.com/2018/10/03/how-to-get-your-k8s-cluster.html)   
[VirtualBox安装部署Ubuntu 16.04 图文详解](https://www.linuxidc.com/Linux/2016-08/134580.htm)  
[download ubuntu 16.04](http://releases.ubuntu.com/16.04/ubuntu-16.04.5-desktop-amd64.iso)   
