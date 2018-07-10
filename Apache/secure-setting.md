# httpd.conf secure policy
### disable server list
```apache
<Directory />
    Options -Indexes    
</Directory>
```

### remove 4XX and 5XX detail error message
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

### disable server banner and signature
```apache
Servertokens Prod  
ServerSignature Off  
```

### disable non-commmon http method 
```apache
TraceEnable Off  
EnableSendfile Off  

<LimitExcept GET POST HEAD>
deny from all
</LimitExcept>
```

### eanble TLS1.2 and redirect all HTTP request to HTTPS
```apache
RedirectMatch ^/(.*)$ https://domain/$1
```

### force httponly and enable secure flag for all request
```apache
Header edit Set-Cookie ^(.*)$ $1;HttpOnly;Secure
```

### avoid clickjacking attacks
```apache
Header always append X-Frame-Options SAMEORIGIN
```

### enable XSS protection
```apache
Header set X-XSS-Protection "1; mode=block"
```

### disable HTTP 1.0 Protocol
```apache
RewriteEngine On
RewriteCond %{THE_REQUEST} !HTTP/1.1$
RewriteRule .* - [F]
```

### shorten time out value
```apache
Timeout 60
```
### disable keepalive (option) if apache is used to proxy or dynamic content (JSP)
```apache
KeepAlive Off
```

# Reference
https://www.acunetix.com/blog/articles/10-tips-secure-apache-installation/  
https://geekflare.com/10-best-practices-to-secure-and-harden-your-apache-web-server/  
https://geekflare.com/apache-web-server-hardening-security/#41-Cookies

