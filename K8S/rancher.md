## install Oracle VM VirtualBox and CentOS
Please refer to my blogs article about VirtualBox installation and setting via below URL.   
https://github.com/jethroau/blogs/tree/master/VirtualBox  

## install docker and set accelerator 
Please refer to official installation guide and below blogs.  
https://blog.csdn.net/kamputer/article/details/79047765  

## check your docker version and centOs version to **exactly** meet Rachner requirement, otherwise you will encounter many problem on Rancher. 
https://www.cnrancher.com/docs/rancher/v2.x/cn/overview/quick-start-guide/  

## Deploy your first app to k8s
```
kubectl verson
kubectl create namespace uat-online
kubectl get namespace

# set default namespace
kubectl config set-context --current --namespace=uat-online
# Validate it
kubectl config view | grep namespace:


```

## Remove node info from etcd and redeploy
```
docker volume rm etcd
```
https://www.jianshu.com/p/3a492440c89b  


## Reference.
https://www.cnrancher.com/
