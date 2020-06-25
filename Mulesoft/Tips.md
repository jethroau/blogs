## For Each Scope 
For Each does not modify the current payload. The output payload is the same as the input.  
https://docs.mulesoft.com/mule-runtime/4.3/for-each-scope-concept 

## mainFlow vs subFlow
1. Payload  
Payload will go thru both mainFlow and subFlow, if subFlow change the payload, then mainFlow response payload will be changed.   

2. Variable  
In FlowReference MainFlow and SubFlow can retrieve both mainFlow and subFlow variable. If variable name is the same, subFlow can change mainFlow variable.   
If Request, subFlow cannot retreieve mainFlow variable.   

3. QueryString and QueryParams  
Subflow cannot retrieve mainFlow QueryString and QueryParams. Mainflow cannot retrieve QueryString and QueryParams if call subflow.   

## Erorr handling
1. on-error-propagate  
don't handle excpetion and throws exception to outside   

2. on-error-continue  
handle this exception and return http code 200  
https://docs.mulesoft.com/mule-runtime/4.3/try-scope-concept  


## Configuring Properties
use Ant-style property placeholders, likes ${xxx.xxx}
1. create a properties file under src/main/resources, such as config.yaml
```
training: 
  host: "127.0.0.1"
  port: "8083"
  basepath: "/"
  protocol: "HTTP"
  
```
2. in Global Elements sheet, create Global configurations and select file "config.yaml"  
3. edit HTTP Listener config, input ${training.port} for port value.  

