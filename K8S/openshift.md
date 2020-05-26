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
## docker login openshift internal registry
```
oc login
## console.osxxx.xxx
## username / password
oc whoami -t
## capture user access token
docker login -u username -p token console.osxxx.xxx
## auth info will be saved at /root/.docker/config.json.
```


## create secret before pulling private image from registry
```
oc create secret docker-registry sosecret --docker-server=docker.io --docker-username=user --docker-password=password --docker-email=email

```
https://github.com/openshift/origin/issues/18932   



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

## Official getting started doc. 
https://docs.openshift.com/enterprise/3.0/getting_started/developers/developers_console.html   

## Reference. 
http://eco.hand-china.com/doc/hip/latest/user_guide/openshift.html 




