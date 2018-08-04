
## create resource group and use docker extention to install docker VM
```
az group create --name Test-dockerVM --location EastAsia

az group deployment create --resource-group Test-dockerVM --template-file ./azuredeploy.json /
    --parameters adminUsername=Login_username adminPassword=******** dnsNameForPublicIP=Your_domain_name /
    vmSize=Standard_B1s

az vm show  --resource-group  Test-dockerVM    --name MyDockerVM  --show-details   --query [fqdns]  --output tsv
````

## reference
https://docs.azure.cn/zh-cn/virtual-machines/linux/dockerextension
