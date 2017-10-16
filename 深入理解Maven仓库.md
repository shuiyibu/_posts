# 一.本地仓库(Local Repository)

本地仓库就是一个本机的目录，这个目录被用来存储我们项目的所有依赖（插件的jar包还有一些其他的文件），简单的说，当你build一个Maven项目的时候，所有的依赖文件都会放在本地仓库里，仓库供所有项目都可以使用

默认情况下，本地仓库在.m2目录，windows下的话就是你的用户名目录下的.m2目录

## 1.更新本地仓库目录

找到你的MAVEN_HOME目录下的conf/setting.xml文件，更新localRepository节点

```
<!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ~/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
-->


<settings>
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ~/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
  <localRepository>D:/maven/repo</localRepository>
```

##  2.保存一下

完成了。新的本地仓库被放在了D:/maven/repo

看一下这个目录

#  二.中央仓库(central repository)

当我们build一个Maven项目的时候，Maven会检查我们的pom.xml文件，来定义项目的依赖，然后Maven会在本地仓库里查找，如果没有找到，就去maven的中央库去下载，地址是`http://search.maven.org/#browse`。看起来是这样的，注意啊，虽然这个是新的中央仓库，但有时候还是会从`http://repo1.maven.org/maven/`这个旧仓库下载东西，不过不要紧，理解就行了

# 三.远程仓库(Remote Respository)

在Maven中，当你在pom.xml中生命的依赖既不在本地库，也不在中央库的时候，就会报错。

## 1.例子

org.jvnet.localizer这个包仅在[java.net的仓库里](https://maven.java.net/content/repositories/public/)有(以前是，现在中央仓库也有了。但理解就行)

```
<dependency>
	<groupId>org.jvnet.localizer</groupId>
	<artifactId>localizer</artifactId>
	<version>1.8</version>
</dependency>
```

当我们build的时候，会失败，并输出未找到错误信息

## 2.声明java.net仓库

为了告诉Maven从远程仓库里获取依赖，我们需要声明一个远程仓库，在pom.xml里这样写

```
<repositories>
	<repository>
		<id>java.net</id>
		<url>https://maven.java.net/content/repositories/public/</url>		</repository>
</repositories>
```

这样，Maven搜索依赖的顺序就是：

1）搜索本地仓库，没有找到，就去第2步，否则退出

2）搜索中央仓库，没有找到，就去第3步，否则退出

3）去[Java](http://lib.csdn.net/base/17).net远程仓库获取，没有找到，就报错，否则退出

补充：JBoss也有个远程仓库，可以如下配置：

```
<project>
	<repositories>
		<repository>
			<id>JBoss repository</id>
			<url>http://repository.jboss.org/nexus/content/groups/public/</url>				</repository>
	</repositories>
</project>
```

