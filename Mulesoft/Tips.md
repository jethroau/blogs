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
   Subflow cannot retrieve mainFlow QueryString and QueryParams. Mainflow cannot retrieve QueryString and QueryParams if call subflow API. 

   
