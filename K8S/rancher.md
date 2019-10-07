## install Oracle VM VirtualBox and CentOS
Please refer to my blogs article about VirtualBox installation and setting via below URL.   
https://github.com/jethroau/blogs/tree/master/VirtualBox  

## install docker and set accelerator 
Please refer to official installation guide and below blogs.  
https://blog.csdn.net/kamputer/article/details/79047765  

## check your docker version and centOs version to **exactly** meet Rachner requirement, otherwise you will encounter many problem on Rancher. 
https://www.cnrancher.com/docs/rancher/v2.x/cn/overview/quick-start-guide/  

## create your first node 
1. login rancher.jethro.io   
2. add cluster, select custom, input cluster name "demo" and select "next"  
3. select 'ectd', 'control', and 'worker' for first node  
4. copy 'docker run' command to one of your node. replace  `--restart=unless-stopped` to ` --restart=always`  
5. wait for node deployment and see "Active" status if everything is successful.   

## use kubectl command in one of docker clients
1. login rancher.jethro.io  
2. select the cluster   
3. click kubeconfig and copy & paste the setting to ~/.kube/config
4. run `kubectl version` and see server status if it is enable to be connected. 


## add your privite register
1. Log into Rancher and configure the default admin password.  
2. Go into the Settings view.  
3. Look for the setting called system-default-registry and choose Edit.  
https://rancher.com/docs/rancher/v2.x/en/installation/air-gap-single-node/config-rancher-for-private-reg/  



## Deploy your first app to k8s
```
kubectl version
kubectl create namespace uat
kubectl get namespace

# set default namespace
kubectl config set-context --current --namespace=uat
# Validate it
kubectl config view | grep namespace:


```

## Remove node info from etcd and redeploy
```
docker ps | awk '{print $1}' | xargs docker stop
docker rm -f $(docker ps -qa)
docker rmi -f $(docker images -q)
docker volume rm $(docker volume ls -q)
```
https://www.jianshu.com/p/3a492440c89b  


## Reference.
https://www.cnrancher.com/  
[私有镜像库构建攻略](https://segmentfault.com/a/1190000007630069)  
