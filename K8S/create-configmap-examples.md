
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
