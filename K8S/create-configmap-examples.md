
### cm-appvars.yaml
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cm-appvars
data:
  apploglevel: info
  appdatadir: /var/data

```


### cmds
```
kubectl create -f cm-appvars.yaml
kubectl get configmiap
kubectl describe configmap cm-appvars

kubectl create configmap nginx.htpasswd --file-file=ngnixhtpasswd
kubectl describe configmap nginx.htpasswd

```

### cm-test-pod2.yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: cm-test-pod2
spec:
  containers:
  - name: cm-test2
    image: busybox
    command: [ "/bin/sh", "-c", "env | grep app" ]
    envFrom:
    - configMapRef:
       name: cm-appvars
  restartPolicy: Never

```

### cmds
```
kubectl create -f cm-test-pod2.yaml
kubectl get pod
kubectl logs cm-test-pod2
```
