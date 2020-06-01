## create project 
in GUI, create new project named [env-system]
```
oc new-project<ProjectName> --description="<description>' --display-name="<DisplayName>"

example: 
oc new-project mobile-app --descrption="Mobile App" --display-name="MOBILE-APP"
```
## create resourceQuota and limitRange for project
```yaml
--- ResourceQuota.yaml 
apiVersion: v1
kind: ResourceQuota
metadata:
  name: namespace-quota
spec:
  hard:
    pods: 16
    limits.cpu: 2
    limits.memory: 8Gi
    
--- LimitRange.yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: container-limit
spec:
  hard:
    requests.cpu: 0.125
    requests.memory: 256Mi
    limits.cpu: 1
    limits.memory: 512Mi
    type: Pod
  limits:
  - default:
      cpu: 1
      memory: 512Mi
    defaultRequest:
      cpu: 0.125
      memory: 256Mi
    type: Container
```

```
oc project mobile-app
oc create -f ResourceQuota.yaml
oc create -f LimitRange.yam
```
## docker login/push openshift internal registry
```
oc login
## console.osxxx.xxx
## username / password

oc whoami -t
## capture user access token

docker login -u username -p token console.openshift.registry
## auth info will be saved at /root/.docker/config.json.

docker tag hello:1.0 console.openshift.registry/hello:lastest
docker push console.openshift.registry/hello:lastest

# list all inputsteam from CLI or can view from Openshift console "Builds => Input Streams"
oc get is -n project-name
```
https://docs.openshift.com/container-platform/3.6/dev_guide/managing_images.html  
https://blog.csdn.net/u012371097/article/details/83746111?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase   

## create secret before pulling private image from registry
```
oc secrets new openshfit2jfrog .dockerconfigjson=/home/users/.docker/config.json
oc secrets link default openshfit2jfrog --for=pull
oc secrets link builder openshfit2jfrog

oc import-image springboot-hello:latest --from=xxxx-docker-registry/springboot-hello:latest --confirm --insecure=true
```
https://docs.openshift.com/container-platform/3.7/dev_guide/managing_images.html#allowing-pods-to-reference-images-across-projects  
https://docs.openshift.com/container-platform/3.6/dev_guide/managing_images.html  
https://github.com/openshift/origin/issues/18932   

## Create / Apply Deploymentconfig

https://docs.openshift.com/enterprise/3.0/dev_guide/deployments.html#start-deployment  


## create pod
in GUI, select image from category

## prepare jenkins
oc update source to project

## create service 
create service for pod

## create route
host, http://xxxx.osapps.xxx.xx

## Developer CLI commands. 
https://docs.openshift.com/dedicated/4/cli_reference/openshift_cli/developer-cli-commands.html 
https://www.jianshu.com/p/6cb3155716ac  

## Official getting started doc. 
https://docs.openshift.com/enterprise/3.0/getting_started/developers/developers_console.html   

## Importing Images from Insecure Registries
```
kind: ImageStream
apiVersion: v1
metadata:
  name: ruby
  annotations:
    openshift.io/image.insecureRepository: "true"
  spec:
    dockerImageRepository: "my.repo.com:5000/myimage"
```
https://docs.openshift.com/enterprise/3.0/architecture/core_concepts/builds_and_image_streams.html   

## Reference. 
http://eco.hand-china.com/doc/hip/latest/user_guide/openshift.html 




