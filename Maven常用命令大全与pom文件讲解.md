# 一、Maven常用命令

## 1.1、Maven 参数

-D 传入属性参数 
-P 使用pom中指定的配置 
-e 显示maven运行出错的信息 
-o 离线执行命令,即不去远程仓库更新包 
-X 显示maven允许的debug信息 
-U 强制去远程参考更新snapshot包 
例如 mvn install -Dmaven.test.skip=true -Poracle 
其他参数可以通过mvn help 获取

## 1.2、maven常用命令

1、mvn clean 

说明: 清理项目生产的临时文件,一般是模块下的target目录

![img](http://img.blog.csdn.net/20150824223257118)

下面来看看目录：

![img](http://img.blog.csdn.net/20150824223326908)

2、mvn package 

说明: 项目打包工具,会在模块下的**==target目录==**生成jar或war等文件，如下运行结果。

![img](http://img.blog.csdn.net/20150824223609591)

生成的文件 如下：

![img](http://img.blog.csdn.net/20150824223639073)

3、mvn test 

说明: 测试命令,或执行src/test/java/下junit的测试用例.

![img](http://img.blog.csdn.net/20150824223741192)

4、mvn install 

说明: 模块安装命令 将打包的的jar/war文件复制到你的**==本地仓库==**中,供其他模块使用 `-Dmaven.test.skip=true` 跳过测试(同时会跳过test compile)

![img](http://img.blog.csdn.net/20150824223836819)

第一个红框是它的输入路径，也是本地仓库的路径

文件如下 ：

![img](http://img.blog.csdn.net/20150824230630165)

5、mvn deploy 

说明: 发布命令。将打包的文件发布到远程参考,提供其他人员进行下载依赖 ,一般是**==发布到公司的私服==**，这里我没配置私服，所以就不演示了。

## 1.3、maven-eclipse-plugin插件

1、mvn eclipse:eclipse 

说明: 生成eclipse配置文件,导入到eclipse开发,如果是使用m2eclipse插件,则可以不用此命令。直接使用插件导入到eclipse进行开放 

注:通过次命令生产的项目,需要在eclipse中配置M2_HOME的命令,指向你的本地仓库文件夹.

![img](http://img.blog.csdn.net/20150824231522715)

来看看生成的结果：。classpath就是字节码

![img](http://img.blog.csdn.net/20150824231540949)

2、mvn eclipse:m2eclipse 

生成eclipse配置文件,该配置文件需依赖eclipse 中有m2eclipse 

-DdownloadSources=true 下载依赖包的源码文件 

-Declipse.addVersionToProjectName=true 添加版本信息到项目名称中 

3、mvn eclipse:clean 

清除eclipse的项目文件

![img](http://img.blog.csdn.net/20150824231306697)

看看文件内容，没有project文件 了

![img](http://img.blog.csdn.net/20150824231404550)

## 1.4、maven-jetty-plugin插件

1、mvn jetty:run 

说明: 可以直接用jetty的服务器运行 注:**==此命令只适用于war的模块==**,即web模块. 

2、mvn archetype:generate 

说明: 模块创建命令, 执行命令后，会提示选择创建项目的模版，这里选18(maven-archetype-quickstart) 

后面会提示你输入groupId(包存放的路径): 

eg:com.lin

提示输入artifactId(模块名称)：

eg:test-core 

提示输入version(版本): 

1.0.0-SNAPSHOT 

提示输入package(指项目中基本的包路径): 

eg:com.lin

提示确认,回车即可

## 1.5、maven-release-plugin插件

说明: 发行版本,可与**==scm工具==**集成,来提供版本管理.不等同与版本控制.允许是必须有goal.两个常用的goal如下: 

1、mvn release:clean 

清理release操作是遗留下来的文件

![img](http://img.blog.csdn.net/20150824233826077)

2、mvn release:branch 

说明: 创建分支,会在分支下创建执行的分支路径 

-DbranchName=xxxx-100317 分支中的名称 

-DupdateBranchVersions=false 是否更新分支的版本信息,默认为false 

-DupdateWorkingCopyVersions=false 是否更新主干的版本信息,默认为true 

3、mvn release:prepare 

创建标记,会有交互过程,提示tag中pom的版本及trunk下的新版本号,每个模块都会询问,默认是最小版本号+1 

-Dtag = 4.4.0 将在tags创建该名称文件夹 

-DdryRun=true 检查各项设置是否正确,可做测试用,会产生一些修改的配置文件信息. 

命令: 

mvn release:perform 

次命令会自动帮我们签出刚才打的tag，然后打包，分发到远程Maven仓库中 

## 1.6、Maven站点报表

1、mvn project-info-reports:dependencies

生成项目依赖的报表

2、mvn dependency:resolve 

查看依赖

![img](http://img.blog.csdn.net/20150824233032385)

查看项目依赖情况 

3、mvn dependency:tree 

打印出项目的整个依赖树 

![img](http://img.blog.csdn.net/20150824233227692)

4、mvn dependency:analyze

帮助你分析依赖关系, 用来取出无用, 重复依赖的好帮手

![img](http://img.blog.csdn.net/20150824233227692)

5、mvn install -X 

追踪依赖的完整轨迹

![img](http://img.blog.csdn.net/20150824233431650)

6、生命周期 

**==resource->compile->process-classes->process-test-resources->test-compile->test->prepare-package->package==** 

resources:resources 绑定在resource处理阶段, 用来将src/main/resources下或者任何指定其他目录下的文件copy到输出目录中 

resources:testResources 将test下的resources目录或者任何指定其他目录copy到test输出目录下 

compiler:testCompile 将测试类编译(包括copy资源文件) 

**==surefire:test 运行测试用例==** 

jar:jar 打jar包

# 二、各种范围

- compile（编译范围）

  compile是==**默认**==的范围；如果没有提供一个范围，那该依赖的范围就是编译范围。编译范围依赖在所有的classpath中可用，同时它们也会被打包。


- provided（已提供范围）

  provided依赖只有在当JDK或者一个容器已提供该依赖之后才使用。例如，如果你开发了一个web应用，你可能在编译classpath中需要可用的Servlet API来编译一个servlet，但是你不会想要在打包好的WAR中包含这个Servlet API；这个Servlet API JAR由你的应用服务器或者servlet容器提供。已提供范围的依赖在编译classpath（不是运行时）可用。它们不是传递性的，也不会被打包。


- runtime（运行时范围）

  runtime依赖在运行和测试系统的时候需要，但在编译的时候不需要。比如，你可能在编译的时候只需要JDBC API JAR，而只有在运行的时候才需要JDBC驱动实现。


- test（测试范围）
- test范围依赖在一般的编译和运行时都不需要，它们只有在测试编译和测试运行阶段可用。


- system（系统范围）

  system范围依赖与provided类似，但是你必须显式的提供一个对于本地系统中JAR文件的路径。这么做是为了允许基于本地对象编译，而这些对象是系统类库的一部分。这样的构件应该是一直可用的，Maven也不会在仓库中去寻找它。。如果你将一个依赖范围设置成系统范围，你必须同时提供一个systemPath元素。注意该范围是不推荐使用的（你应该一直尽量去从公共或定制的Maven仓库中引用依赖）。

# 三、POM文件讲解

POM全称是Project Object Model，即项目对象模型。pom.xml是maven的项目描述文件，它类似于ant的project.xml文件。pom.xml文件以xml的 形式描述项目的信息，包括项目名称、版本、项目id、项目的依赖关系、编译环境、持续集成、项目团队、贡献管理、生成报表等等。总之，它包含了所有的项目 信息。

3.2.1. pom.xml的基本配置

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  
  <groupId>com.lin.learning</groupId>  
  <artifactId>maven-hellowrold</artifactId>  
  <version>0.0.1-SNAPSHOT</version>  
  <packaging>jar</packaging>  
  
  <name>maven-hellowrold</name>  
  <url>http://maven.apache.org</url>  
  
  <properties>  
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
  </properties>  
  
</project>  
```

- `modelVersion` 描述这个POM文件是遵从哪个版本的项目描述符。
- `groupId` 针对一个项目的普遍唯一识别符。通常用一个完全正确的包的名字来与其他项目的类似名字来进行区分（比如：org.apache.maven)。
- `artifactId` 在给定groupID 的group里面为artifact 指定的标识符是唯一的 ， artifact 代表的是被制作或者被一个project应用的组件(产出物)。
- `version` 当前项目产生的artifact的版本以上4个元素缺一不可，其中**groupId, artifactId, version描述依赖的项目唯一标志**。

## 1. pom.xml文件结构

```
<project>  
<modelVersion>4.0.0</modelVersion>  
<!- The Basics 项目的基本信息->  
<groupId>...</groupId>  
<artifactId>...</artifactId>  
<version>...</version>  
<packaging>...</packaging>  
<dependencies>...</dependencies>  
<parent>...</parent>  
<dependencyManagement>...</dependencyManagement>  
<modules>...</modules>  
<properties>...</properties>  
<!- Build Settings 项目的编译设置->  
<build>...</build>  
<reporting>...</reporting>  
<!- More Project Information 其它项目信息 ->  
<name>...</name>  
<description>...</description>  
<url>...</url>  
<inceptionYear>...</inceptionYear>  
<licenses>...</licenses>  
<organization>...</organization>  
<developers>...</developers>  
<contributors>...</contributors>  
<!-- Environment Settings ->  
<issueManagement>...</issueManagement>  
<ciManagement>...</ciManagement>  
<mailingLists>...</mailingLists>   
<scm>...</scm>  
<prerequisites>...</prerequisites>  
<repositories>...</repositories>  
<pluginRepositories>...</pluginRepositories>  
<distributionManagement>...</distributionManagement>  
<profiles>...</profiles>  
</project>  
```

project是pom.xml的根节点，至于其它元素请参考POM Reference

# 2.、POM很重要的3个关系

POM有3个很重要的关系：依赖、继承、合成。

2.1. 依赖关系

如果想依赖一个maven库中没有的一个jar包，方法很简单，就是先将此jar包使用以下的命令安装到本地maven库中：

```
mvn install:install-file -Dfile=my.jar -DgroupId=mygroup -DartifactId=myartifactId -Dversion=1
```

再把依赖关系加进去即可。

2.2. 继承关系

另一个强大的变化, maven带来的是项目继承。

2.2.1. 定义父项目

```xml
<project>  
<modelVersion>4.0.0</modelVersion>  
<groupId>com.mygroup </groupId>  
<artifactId>my-parent</artifactId>  
<version>2.0</version>  
<packaging>pom</packaging>  
</project>
```

packaging 类型，定义值为 pom用于定义为parent和合成多个项目。 当然我们创建的maven项目的pom都继承maven的super pom， 如果想看项目(父或子)的完全的pom结构，可以运行：

`mvn help:effective-pom`

就可以了。

2.2.2. 子项目配置

```
<project>  
<modelVersion>4.0.0</modelVersion>  
<groupId>com.mygroup </groupId>  
<artifactId>my-child-project</artifactId>  
<parent>  
<groupId>com.mygroup </groupId>  
<artifactId>my-parent</artifactId>  
<version>2.0</version>  
<relativePath>../my-parent</relativePath>  
</parent>  
</project> 
```

2.3. 合成关系

一个项目有多个模块，也叫做**多重模块**，或者合成项目。 如下的定义：

```xml
<project>  
<modelVersion>4.0.0</modelVersion>  
<groupId>com.mygroup </groupId>  
<artifactId>my-parent</artifactId>  
<version>2.0</version>  
<modules>  
<module>my-child-project1<module>  
<module>my-child-project2<module>  
</modules>  
</project> 
```

其中module 描述的是子项目的相对路径 。

2.4. dependencyManagement和Profile

Maven 还我们提供了一个dependencyManagement元素，用来提供了一种方式来统一依赖版本号。**dependencyManagement元素一般用在顶层的父POM**。使用pom.xml中的dependencyManagement元素能让你在**子项目中引用一个依赖而不用显式的列出版本号**。 Maven会沿着父子层次向上走，直到找到一个拥有dependencyManagement元素的项目，然后它就会使用在这个 dependencyManagement元素中指定的版本号，这样就解决了修改依赖版本号不完全的问题。

Maven的Profile元素可以为一个特殊的环境自定义一个特殊的构建，使得不同环境间构建的可移植性成为可能。比如要使用 production profile来运行mvn install，你需要在命令行传入-Pproduction参数，这里production是profile的id。要验证production profile覆盖了默认的Compiler插件配置，可以像这样以开启调试输入(-X) 的方式运行Maven。

