---

---

 

# [maven 的基本配置及个人理解](http://blog.csdn.net/maguanghui_2012/article/details/51544472)

maven的安装包的`conf`包下有`setting.xml`文件，此文件为maven的配置文件，用来定义本地仓库和远程仓库的基本配置

- `<localRepository> </localRepository> ` 定义本地仓库

- `<server><username></username><password></password></server> ` 用于定义远程server  这个在项目中会经常看到，我们会在一台服务器用来发布项目，并把本地的这个文件的`<server>`设置为远程的server，这样就可以达到项目组的个人跟远程server jar包共享。

  [^eclipsezhonc]: eclipse中怎么设置

  ​

maven的常用命令

- 创建一个简单的Java工程：`mvn archetype:create -DgroupId=com.mycompany.example -DartifactId=Example`
- 创建一个java的web工程：`mvn archetype:create -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-webapp -DgroupId=com.mycompany.app -DartifactId=my-webapp`
- 打包：`mvn package`
- 编译：`mvn compile`
- 编译测试程序：`mvn test-compile`
- 清空：`mvn clean`
- 运行测试：`mvn test`
- 生成站点目录: `mvn site `
- ==**生成站点目录并发布**==：`mvn site-deploy`
- **==安装当前工程的输出文件到本地仓库==**: `mvn install`
- ==**安装指定文件到本地仓库**==：`mvn install:install-file -DgroupId=<groupId> -DartifactId=<artifactId> -Dversion=1.0.0 -Dpackaging=jar -Dfile=<myfile.jar>`
- 查看实际pom信息: mvn help:effective-pom
- 分析项目的依赖信息：mvn dependency:analyze 或 mvn dependency:tree
- ==**跳过测试运行maven任务**==：    mvn -Dmaven.test.skip=true XXX
- 生成eclipse项目文件: mvn eclipse:eclipse



下面主要介绍我对某几个的理解，`mvn clean` 会把项目编译后的都clean掉，`mvn compile` 会把Java文件编译为class文件，`mvn install` 不仅编译还有打包 假如你要install又要跳过测试 `mvn install -Dmaven.test.skip=true`。当然，如果要集成在eclipse里，你在eclipse里只需要把skip test那勾选就行了，不过eclipse也只是在你勾选后把-Dmaven.test.skip设为true，原理是一样的。

另外要讲的是pom文件, 最后面是我搭的SSH 的一个框架，里面的jar包基本包括SSH所需要的。

**POM关系：**
主要为依赖，继承，合成
**依赖关系：**

```xml
<dependencies>  
    <dependency>  
      <groupId>junit</groupId>  
      <artifactId>junit</artifactId>  
      <version>4.0</version>  
      <type>jar</type>  
      <scope>test</scope>  
      <optional>true</optional>  
    </dependency>  
    ...  
</dependencies>  
```

像这样的直接‘导入’包的是依赖关系。

还有一种依赖关系也是常见到的：告诉maven你只包括指定的项目，不包括相关的依赖，此因素主要用于解决版本冲突问题  

```
<dependency>  
     <groupId>org.apache.maven</groupId>  
      <artifactId>maven-embedder</artifactId>  
      <version>2.0</version>  
      <exclusions>  
        <exclusion>  
          <groupId>org.apache.maven</groupId>  
          <artifactId>maven-core</artifactId>  
        </exclusion>  
      </exclusions>  
</dependency>  
```

表示项目`maven-embedder`需要项目`maven-core`，但我们不想引用`maven-core  `

**继承关系：**

这个也是企业应用必不可少的。一般项目不会只有一个jar包，比如你要发布一个web应用，会有好多模块。

定义父项目 

```xml
<project>  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>org.codehaus.mojo</groupId>  
  <artifactId>my-parent</artifactId>  
  <version>2.0</version>  
  <packaging>pom</packaging>  
</project>  
```

packaging 类型，需要pom用于parent和合成多个项目。我们需要增加相应的值给父pom，用于子项目继承。

子项目配置  

```
<project>  
  <modelVersion>4.0.0</modelVersion>  
  <parent>  
    <groupId>org.codehaus.mojo</groupId>  
    <artifactId>my-parent</artifactId>  
    <version>2.0</version>  
    <relativePath>../my-parent</relativePath>  
  </parent>  
  <artifactId>my-project</artifactId>  
</project>  
```

`relativePath`可以不需要，但是用于指明parent的目录，用于快速查询。  

dependencyManagement：  
用于父项目配置共同的依赖关系，主要配置依赖包相同因素，如版本，scope。  

这里注意此继承不可以循环，比如a继承b，b继承c，c继承d，d继承a 这样是错误的。

**合成关系：**

合成（或者多个模块）  ，一个项目有多个模块，也叫做多重模块，或者合成项目。  如下的定义： 

```
<project>  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>org.codehaus.mojo</groupId>  
  <artifactId>my-parent</artifactId>  
  <version>2.0</version>  
  <modules>  
    <module>my-project1<module>  
    <module>my-project2<module>  
  </modules>  
</project>  
```

完整的ssh 的pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>Maven_Demo_02</groupId>  
  <artifactId>Maven_Demo_02</artifactId>  
  <packaging>war</packaging>  
  <version>0.0.1-SNAPSHOT</version>  
  <name>Maven_Demo_02 Maven Webapp</name>  
  <url>http://maven.apache.org</url>  
    
  <properties>  
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
  <spring.version>3.1.4.RELEASE</spring.version>  
  <hibernate-version>4.1.9.Final</hibernate-version>  
  <hibernate-search-version>4.2.0.Beta2</hibernate-search-version>  
  <struts-version>2.3.8</struts-version>  
  <commons-dbcp-version>1.4</commons-dbcp-version>  
  <cglib-version>2.2.2</cglib-version>  
  <aspectjweaver-version>1.7.1</aspectjweaver-version>  
  <jstl-version>1.2</jstl-version>  
  <servlet-version>3.0-alpha-1</servlet-version>  
  <jsp-version>2.2.1-b03</jsp-version>  
  <guava-version>13.0.1</guava-version>  
  <slf4j-nop-version>1.7.2</slf4j-nop-version>  
  <log4j-version>1.2.17</log4j-version>  
  <junit-version>4.11</junit-version>  
  <<a href="http://lib.csdn.net/base/14" class="replace_word" title="undefined" target="_blank" style="color:#df3434; font-weight:bold;">mysql</a>-connector-version>5.1.22</mysql-connector-version>  
</properties>  
  
  
  <dependencies>  
  
  <!-- spring start -->  
   <dependency>  
    <groupId>org.springframework</groupId>  
    <artifactId>spring-webmvc</artifactId>  
    <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-aop</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-aspects</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-asm</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
              
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-beans</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-context</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-context-support</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-core</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-expression</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-instrument-tomcat</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-instrument</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-jdbc</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-jms</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-orm</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-oxm</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-test</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-tx</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.springframework</groupId>  
   <artifactId>spring-web</artifactId>  
   <version>${spring.version}</version>  
  </dependency>  
  <!-- springs end -->  
    
    
  <!-- Struts2 required  start-->  
  <dependency>  
   <groupId>org.apache.struts</groupId>  
   <artifactId>struts2-core</artifactId>  
   <version>${struts-version}</version>  
  </dependency>  
  <dependency>  
   <groupId>org.apache.struts.xwork</groupId>  
   <artifactId>xwork-core</artifactId>  
   <version>${struts-version}</version>  
  </dependency>  
         <dependency>  
   <groupId>javassist</groupId>  
   <artifactId>javassist</artifactId>  
   <version>3.12.1.GA</version>  
  </dependency>  
         <dependency>  
   <groupId>ognl</groupId>  
   <artifactId>ognl</artifactId>  
   <version>3.0.6</version>  
  </dependency>   
         <dependency>  
   <groupId>org.freemarker</groupId>  
   <artifactId>freemarker</artifactId>  
   <version>2.3.19</version>  
  </dependency>  
  <dependency>  
   <groupId>commons-dbcp</groupId>  
   <artifactId>commons-dbcp</artifactId>  
   <version>${commons-dbcp-version}</version>  
  </dependency>  
  <dependency>  
   <groupId>commons-logging</groupId>  
   <artifactId>commons-logging</artifactId>  
   <version>1.1.1</version>  
  </dependency>  
        <dependency>  
   <groupId>commons-lang</groupId>  
   <artifactId>commons-lang</artifactId>  
   <version>2.6</version>  
  </dependency>  
  <dependency>  
   <groupId>commons-io</groupId>  
   <artifactId>commons-io</artifactId>  
   <version>2.4</version>  
  </dependency>  
  <dependency>  
   <groupId>commons-fileupload</groupId>  
   <artifactId>commons-fileupload</artifactId>  
   <version>1.2.2</version>  
  </dependency>  
              
  <!-- Struts2 required End  -->  
    
    
  <!-- Hibernate required-->  
  <dependency>  
   <groupId>org.hibernate</groupId>  
   <artifactId>hibernate-core</artifactId>  
   <version>${hibernate-version}</version>  
  </dependency>  
  
  
    
  
  <dependency>  
   <groupId>org.apache.struts</groupId>  
   <artifactId>struts2-spring-plugin</artifactId>  
   <version>${struts-version}</version>  
  <..  
[2013/4/19 15:14:22] paul.zhang: <dependency>  
   <groupId>org.apache.struts</groupId>  
   <artifactId>struts2-convention-plugin</artifactId>  
   <version>${struts-version}</version>  
  </dependency>  
  
  <dependency>  
   <groupId>org.apache.struts</groupId>  
   <artifactId>struts2-json-plugin</artifactId>  
   <version>${struts-version}</version>  
  </dependency>  
  
  <dependency>  
   <groupId>javax.servlet</groupId>  
   <artifactId>jstl</artifactId>  
   <version>${jstl-version}</version>  
  </dependency>  
  
  <dependency>  
   <groupId>javax.servlet</groupId>  
   <artifactId>servlet-api</artifactId>  
   <version>${servlet-version}</version>  
   <scope>provided</scope>  
  </dependency>  
  
  <dependency>  
   <groupId>javax.servlet.jsp</groupId>  
   <artifactId>jsp-api</artifactId>  
   <version>${jsp-version}</version>  
   <scope>provided</scope>  
  </dependency>  
  
  
  <!-- log4j -->  
  <!-- <dependency>  
   <groupId>log4j</groupId>  
   <artifactId>log4j</artifactId>  
   <version>${log4j-version}</version>  
  </dependency>  
-->  
  <!-- JUnit -->  
  <dependency>  
   <groupId>junit</groupId>  
   <artifactId>junit</artifactId>  
   <version>${junit-version}</version>  
   <scope>test</scope>  
  </dependency>  
    
  <dependency>  
   <groupId>mysql</groupId>  
   <artifactId>mysql-connector-java</artifactId>  
   <version>5.1.23</version>  
  </dependency>   
        <dependency>    
      <groupId>org.aspectj</groupId>    
      <artifactId>aspectjweaver</artifactId>    
      <version>1.6.12</version>    
  </dependency>   
  <dependency>  
   <groupId>org.apache.activemq</groupId>  
   <artifactId>activemq-core</artifactId>  
   <version>5.7.0</version>  
  </dependency>  
        <dependency>  
   <groupId>javax.mail</groupId>  
   <artifactId>mail</artifactId>  
   <version>1.4.6</version>  
  </dependency>  
</dependencies>  
    
  <build>  
    <finalName>Maven_Demo_01</finalName>  
  </build>  
</project>  
```



