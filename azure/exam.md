# AZ-203
[Developer learning path](https://docs.microsoft.com/en-us/learn/browse/?products=azure&roles=developer&resource_type=learning%20path)  
[Self-paced diagram](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWtQqM)  

> 考试范围
> https://www.microsoft.com/zh-cn/learning/exam-AZ-203.aspx   

## Azure fundmential
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
