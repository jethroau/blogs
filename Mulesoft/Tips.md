## For Each Scope 
For Each does not modify the current payload. The output payload is the same as the input.  
https://docs.mulesoft.com/mule-runtime/4.3/for-each-scope-concept 

## mainFlow vs subFlow
In HTTP Request, subFlow can retrieve mainFlow `payload` only.   
After HTTP request, mainFlow can still retrieve subFlow `payload` and mainFlow `variable`. 
In FlowReference, MainFlow and SubFlow can retrieve `payload, variable and QueryString/Params`.   

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

## JMS publish vs publish consume
1. `publish` operation can be used also to publish a Message to a given Topic destination and `NO wait` for a reply
2. `publish-consume` operation lets you publish a message to any destination, and then `wait for a reply` on a different destination.  
https://docs.mulesoft.com/jms-connector/1.7/jms-publish-consume  
