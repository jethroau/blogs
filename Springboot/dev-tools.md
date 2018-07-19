## enable dev tools
```xml
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
</dependency>
```

## kill windows process created by dev tools
```
netstat -ano | findstr "8010"

taskkill /F /PID 3080
```
reference: https://therealdanvega.com/blog/2015/04/16/windows-kill-process-by-port-number
