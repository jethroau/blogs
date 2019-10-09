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


## create new namespace 
```
kubectl version
kubectl create namespace uat
kubectl get namespace

# set default namespace
kubectl config set-context --current --namespace=uat
# Validate it
kubectl config view | grep namespace:
```

## prepare your deployment.yaml and service.yaml
deployment.yaml
```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
    name: web-hello ##system name
spec:
    replicas: 1
    template:
        metadata:
          labels:
            app: ja-springboot-hello
            release: "20190907"
            environment: "uat"
        spec:
          containers:
            - name: ja-springboot-hello
              image: repo.jethro.io/demo/ja-springboot-hello:1.0
              ports:
                 - containerPort: 80

```

servie.yaml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-hello-service-on-node3
  namespace: default
spec:
  type: NodePort
  ports:
  - port: 80
    nodePort: 30003
  selector:
    app: ja-springboot-hello
```

## Deploy your first app to k8s
```
# ensure your springboot docker image was uploaded to haror, refer to springboot & docker chapter. 
kubectl create -f deployment.yaml
kubectl get pod
kubectl get deployment

kubectl create -f service.yaml
```
open browser and enter http://rancher-node.jethro.io:30003/sayHello and it will display  
"Hello world!"  


## Remove node info from etcd and redeploy
```
docker ps | awk '{print $1}' | xargs docker stop
docker rm -f $(docker ps -qa)
docker rmi -f $(docker images -q)
docker volume rm $(docker volume ls -q)
```
https://www.jianshu.com/p/3a492440c89b  

## Clarify k8s delopyment, service and network ip concept 
Deployment = ReplicaSet = RC 用于创建和管理POD, 他们yaml中的template等同于POD的yaml.   
POD: POD IP + Container Port = endpoint, IP随着POD的创建和消忙而改变的。  
Service：ClusterIP 一旦service被创建，ClusterIP在整个Service生命周期内不会发生改变。是K8S内部全局唯一的虚拟IP  
NodePort: 提供给外部访问的途径，　一旦创建，每个Node上的NodePort都提过这个服务。前面只需有一个loadbalancer做负载均衡。可以是Nginx或者HAProxy. 


## Reference.
https://www.cnrancher.com/  
[私有镜像库构建攻略](https://segmentfault.com/a/1190000007630069)  
