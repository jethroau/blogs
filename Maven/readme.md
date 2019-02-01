
## set local mirror
/usr/share/maven/conf/settings.xml
```xml
  <mirrors>
    <mirror>
      <id>aliyun-repo</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun maven repo</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
  </mirrors>

```
