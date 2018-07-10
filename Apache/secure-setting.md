# httpd.conf secure policy
## disable server list
```apache
<Directory />
    Options -Indexes    
</Directory>
```

## remove 4XX and 5XX detail error message
```apache
ErrorDocument 400  "Server ERROR!"
ErrorDocument 401  "Server ERROR!"
ErrorDocument 403  "Server ERROR!"
ErrorDocument 404  "Server ERROR!"  
#4xx...
ErrorDocument 500  "Server ERROR!"  
ErrorDocument 501  "Server ERROR!"  
ErrorDocument 502  "Server ERROR!"  
#5xx..
```

## disable server banner and signature
```apache
Servertokens Prod  
ServerSignature Off  
```

## disable non-commmon http method 
```apache
TraceEnable Off  
EnableSendfile Off  
```

# reference
https://www.acunetix.com/blog/articles/10-tips-secure-apache-installation/  
https://geekflare.com/10-best-practices-to-secure-and-harden-your-apache-web-server/  

