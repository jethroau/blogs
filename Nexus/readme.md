
## install as a windows service 
open cmd and click “Run as Administrator”.
```
nexus /install
```

## run in dos directly
```
nexus /run
```

## Admin page
> enter http://localhost:8081  
> input admin/admin123

## Private Maven Repo
1.Create maven hosted
1.1 create repository,  
1.2 select maven hosted, 
1.3 input repo name "my-hosted".  

2.Create maven proxy
2.1 create repository
2.2 sleect maven proxy
2.3 input proxy name "aliyun"
2.4 input URL http://maven.aliyun.com/nexus/content/groups/public

3. Create maven group 
3.1 create a group "my-repo"
3.2 add "my-hosted" and your proxy to this group.

4. vi $maven_home/setting.xml
4.1 copy your repo URL http://localhost:8081/repository/my-repo/
4.2 paste URL and add below setting. 
```xml
 <server>
         <id>my-repo</id>
         <username>admin</username>
         <password>admin123</password>
 </server>
 
 <mirror>
       <id>my-repo</id>
       <mirrorOf>*</mirrorOf>
       <url>http://localhost:8081/repository/my-repo/</url>
 </mirror>
```

## reference
[Windows10 下安装 Nexus OSS 3.xx](https://blog.csdn.net/rekadowney/article/details/52492587#%E8%BF%90%E8%A1%8C%E5%B9%B6%E5%AE%89%E8%A3%85nexus%E7%9A%84windows%E6%9C%8D%E5%8A%A1)  
[Windows: OpenSCManager failed](https://support.sonatype.com/hc/en-us/articles/213464718-Windows-OpenSCManager-failed-Access-is-denied-0x5-errors-when-starting-Nexus)    
