# AZ-203
[Developer learning path](https://docs.microsoft.com/en-us/learn/browse/?products=azure&roles=developer&resource_type=learning%20path)  
[Self-paced diagram](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWtQqM)  

> 考试范围
> https://www.microsoft.com/zh-cn/learning/exam-AZ-203.aspx   

## Azure fundmental
### create VM
```
az vm create \
--name myVM1 \
-g 56c7a8f8-185d-4560-8628-bb0248fcfd3c \
--image UbuntuLTS \
--location southeastasia \
--size Standard_DS2_v2  \
--admin-username azureuser \
--generate-ssh-keys
```
when VM is ready, we may receive following repsonse from Azure CLI
```json
{
  "fqdns": "",
  "id": "/subscriptions/d59d31dd-3c6b-42fa-bec0-4bfcb1a9ad24/resourceGroups/56c7a8f8-185d-4560-8628-bb0248fcfd3c/providers/Microsoft.Compute/virtualMachines/myVM1",
  "location": "southeastasia",
  "macAddress": "00-0D-3A-A2-C0-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "104.215.156.104",
  "resourceGroup": "56c7a8f8-185d-4560-8628-bb0248fcfd3c",
  "zones": ""
}
```
### install Nginx in the VM 
```
az vm extension set \
-g 56c7a8f8-185d-4560-8628-bb0248fcfd3c \
--vm-name myVM1 \
--name customScript \
--publisher Microsoft.Azure.Extensions \
--settings "{'fileUris':['https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh']}" \
--protected-settings "{'commandToExecute': './configure-nginx.sh'}"
```

### open 80 port in firewall
```
az vm open-port \
--name myVM \
--resource-group 56c7a8f8-185d-4560-8628-bb0248fcfd3c \
--port 80
```


