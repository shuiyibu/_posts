Maven Tomcat7自动部署



1、配置tomcat-users.xml文件

在tomcat安装目录下找到tomcat-users.xml文件。该文件路径为【tomcat安装根目录】/conf/

修改文件内容，增加下列内容：

```
<tomcat-users>  
<role rolename="manager"/>  
<role rolename="admin"/>  
<role rolename="manager-gui"/>  
 <role rolename="manager-script"/>  
<user username = "admin" password = "password" roles = "admin,manager,manager-gui,manager-script" />  
</tomcat-users>
```

启动tomcat8.5，然后访问 <http://localhost:8080/manager/html>，输入admin/password，如果出现以下界面，表示tomcat一切OK

![img](http://img.blog.csdn.net/20150702192642490)

2、配置maven 的setting.xml 文件

[^]: eclipse中怎么配置



```
<server>  
 <id>tomcat8.5</id>  
 <username>admin</username>  
 <password>password</password>  
</server> 
```

3、配置项目pom.xml文件

```
<plugin>  
    <groupId>org.codehaus.mojo</groupId>  
    <artifactId>tomcat-maven-plugin</artifactId>  
    <version>1.1</version>  
    <configuration>  
        <url>http://localhost:8080/manager/text</url>  
        <server>tomcat7</server>  
        <username>admin</username>  
        <password>password</password>  
        <ignorePackaging>true</ignorePackaging>    
    </configuration>  
</plugin>
```

注:此处的url 注意是xxx/manager/text 并非是 xxx/manager/html 原因是我用的tomcat 是tomcat7 的版本

4、cmd运行

先进入到项目所在的目录，然后运行

```
mvn tomcat:redeploy  
```

![img](http://img.blog.csdn.net/20150702193216908)

最终结果：

其中只有system-web是web项目，其它都不是，只是一些依赖项目

![img](http://img.blog.csdn.net/20150702193341193)

在目录D:\[Java](http://lib.csdn.net/base/17)\Tool\apache-tomcat-7.0.62\webapps可以找到发布好的文件

![img](http://img.blog.csdn.net/20150702193614695)

浏览器输入：http://localhost:8080/system-web/

![img](http://img.blog.csdn.net/20150702194054884)

常见错误排除：

1.Connection refused错误

报错信息如下：