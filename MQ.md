# MQ的基本概念



> mq是通讯中间件。他的作用是省去开发人员开发通讯工具的时间，节省开发成本，提高开发效率。



## 队列管理器

队列管理器是MQ系统中最上层的一个概念，由它为我们提供基于队列的消息服务。简单的说就是一个大容器的管理员，这个大容器里放了很多东西。

## 消息

在MQ中，我们把应用程序交由MQ传输的==**数据**==定义为`消息`，我们可以定义消息的内容并对消息进行广义的理解，比如：用户的各种类型的数据文件，某个应用向其它应用发出的处理请求等都可以作为消息。消息有两部分组成：

- `消息描述符`(Message Discription或Message Header)，描述消息的特征，如：消息的优先级、生命周期、消息Id等；
- `消息体`(Message Body)，即用户数据部分。在MQ中，消息分为两种类型，非永久性(non-persistent)消息和永久性(persistent)消息：
  - `非永久性消息`是存储在内存中的，它是为了提高性能而设计的，当系统掉电或MQ队列管理器重新启动时，将不可恢复。当用户对消息的可靠性要求不高，而侧重系统的性能表现时，可以采用该种类型的消息，如：当发布股票信息时，由于股票信息是不断更新的，我们可能每若干秒就会发布一次，新的消息会不断覆盖旧的消息。
  - `永久性消息`是存储在硬盘上，并且纪录数据日志的，它具有高可靠性，在网络和系统发生故障等情况下都能确保消息不丢、不重。

## 队列

队列是消息的安全存放地，队列存储消息直到它被应用程序处理。通俗的理解就是大容器里的东西，存放消息的盒子。消息队列以下述方式工作：

- 程序A形成对消息队列系统的调用，此调用告知消息队列系统，消息准备好了投向程序B；
- 消息队列系统发送此消息到程序B驻留处的系统，并将它放到程序B的队列中；
- 适当时间后，程序B从它的队列中读此消息，并处理此信息。

在MQ中，队列分为很多种类型，其中包括：**本地队列、远程队列、模板队列、动态队列、别名队列**等。

本地队列又分为普通本地队列和传输队列，普通本地队列是应用程序通过API对其进行读写操作的队列；传输队列可以理解为存储-转发队列，比如：我们将某个消息交给MQ系统发送到远程主机，而此时网络发生故障，MQ将把消息放在传输队列中暂存，当网络恢复时，再发往远端目的地。

远程队列是目的队列在本地的定义，它类似一个地址指针，指向远程主机上的某个目的队列，它仅仅是个定义，不真正占用磁盘存储空间。

模板队列和动态队列是MQ的一个特色，它的一个典型用途是用作系统的可扩展性考虑。我们可以创建一个模板队列，当今后需要新增队列时，每打开一个模板队列，MQ便会自动生成一个动态队列，我们还可以指定该动态队列为临时队列或者是永久队列，若为临时队列我们可以在关闭它的同时将它删除，相反，若为永久队列，我们可以将它永久保留，为我所用。

 

## 通道

通道是MQ系统中队列管理器之间传递消息的管道，它是建立在物理的网络连接之上的一个逻辑概念，也是MQ产品的精华。s通俗的理解就是大容器和大容器之间，程序和容器之间进行通讯的途径。

在MQ中，主要有三大类通道类型，即消息通道，MQI通道和Cluster通道。

- 消息通道是用于在MQ的服务器和服务器之间传输消息的，需要强调指出的是，该通道是单向的，它又有发送(sender), 接收(receive), 请求者(requestor), 服务者(server)等不同类型，供用户在不同情况下使用。
- MQI通道是MQ Client和MQ Server之间通讯和传输消息用的，与消息通道不同，它的传输是双向的。
- 群集(Cluster)通道是位于同一个MQ 群集内部的队列管理器之间通讯使用的。



mq的通讯方式有两种，通俗的说就是mq之间进行通讯，开发的程序和mq之间的通讯。

- mq之间进行通讯：通过发送接收通道建立tcp连接进行消息传输，称为server对server
- 开发的程序和mq之间的通讯：通过服务器连接通道进行传输，client对server



## MQ的工作原理

![img](file:///C:/Users/user/AppData/Local/Temp/msohtmlclip1/01/clip_image001.gif)

如图所示：

首先来看本地通讯的情况，应用程序A和应用程序B运行于同一系统A，它们之间可以借助消息队列技术进行彼此的通讯：应用程序A向队列1发送一条信息，而当应用程序B需要时就可以得到该信息。

其次是远程通讯的情况，如果信息传输的目标改为在系统B上的应用程序C，这种变化不会对应用程序A产生影响，应用程序A向队列2发送一条信息，系统A的MQ发现Q2所指向的目的队列实际上位于系统B，它将信息放到本地的一个特殊队列－传输队列(Transmission Queue)。我们建立一条从系统A到系统B的消息通道，消息通道代理将从传输队列中读取消息，并传递这条信息到系统B，然后等待确认。只有MQ接到系统B成功收到信息的确认之后，它才从传输队列中真正将该信息删除。如果通讯线路不通，或系统B不在运行，信息会留在传输队列中，直到被成功地传送到目的地。这是MQ最基本而最重要的技术--确保信息传输，并且是一次且仅一次(once-and-only-once)的传递。

 

 

## MQ的基本配置举例

 

如何配置两台mq使之相互进行通讯？

首先要规划好两个队列管理器之间使用的ip和端口，假设我们使用

ip            端口

192.168.0.1   1414

192.168.0.2   1415

### 第一步 建立队列管理器

`crtmqm -lc -lf 100 -lp 3 -ls 3 QM1 `

解释下：

-lc 是采用循环日志

-lf 是每块日志的大小，4k为单位的，100就是100*4k

-lp 是主逻辑日志的数量

-ls 是辅逻辑日志的数量

QM1 是队列管理器名称 

创建队列管理器时，应考虑的因素主要有：

1） 队列管理器的日志类型以及日志文件的大小和个数，要根据用户数据量的大小、各个队列上的消息总容量，来计算日志的总容量，以免在系统运行过程中出现日志写满的情况；

2） 应该为队列管理器指定和建立**死信队列**；

3） 对最多打开句柄数MAXHANDS（缺省为256，如果您需要多于256个应用程序同时连接队列管理器，应增大该值），最大消息长度MAXMSGL，最多的未提交的消息个数MAXUMSGS属性（缺省为10000，如果您使用了消息分段或分组，某个大消息的分段个数超过了10000，应增大该值）的考虑。

### 第二步 启动队列管理器

- 要启动队列管理器，输入：`strmqm` 
  会有一条消息告诉您队列管理器已启动。 

- 要启动 MQSC 命令，输入： `runmqsc `

  当 MQSC 会话启动后会有一条消息告诉您。MQSC 没有命令提示符。 

### 第三步 定义队列管理器中的队列和通道等

- 先运行`runmqsc` QM1首先要保证运行该命令的用户属于mqm组

- 运行完后进入mq命令窗口

-  定义本地队列 `def ql(QL1)`

   先解释什么是本地队列，然后解释命令的含义（以下同）

   本地队列是存储信息的盒子，用户可以从本地队列里取消息，对方发送消息的目的地也是本地队列。

   def是 define的缩写，mq支持一些命令的缩写；ql是queue local的缩写，表示本地队列，括号内是本地队列名

- 定义远程队列 `def qr(QR1)`

  远程队列是相对于本地队列的，当用户希望往另一个队列管理器发消息的时候，配置好远程队列，用户直接放消息到该队列就可以，mq会传输到另一方的本地队列中。

  以上面的例子说明，当我们把消息放入该远程队列后，消息会传输到QM2队列管理器中的QL2队列中。

  - qr queue remote的缩写 
  - rname 指定的远程队列管理器上的队列名
  - rqname 远程队列管理器
  - xmitq 所要用的传输队列


- 定义传输队列 `def ql(QT1)`

 ```
defql(QT1) usage(xmitq) trigger trigtype(first) initq
(system.channel.initq)trigdata(QM1.QM2)
 ```

- 传输队列是传输的介质，消息是通过传输队列进行传输的。
  - usage 用途xmitq是传输队列
  - trigger 消息触发开关
  - trigtype 触发类型第一条消息触发
  - initq    初始队列
  - trigdata 触发数据

对于队列的属性，应该考虑的因素主要有：

1） 永久性和非永久性设置：尤其要注意的是DEFPSIST属性的缺省值为No，若要保证消息的安全可靠，必须将其设置为Yes；

2） 对于本地队列和传输队列,要考虑队列的最大深度MAXDEPTH(缺省为5000，应根据实际情况计算该值)，队列中每个消息的最大字节数MAXMSGL的配置。

- 定义发送通道 `def chl(QM1.QM2)`

```
def chl(QM1.QM2) chltype(sdr)conname('192.168.0.2(1415)')  trptype (tcp) xmitq(QT1) 
```

- 发送通道就相当于建立一个tcp的连接

  - chl channel的缩写
  - chltype 通道类型sdr是发送通道
  - conname 连接名包括对方的ip和端口
  - trptype 通讯类型tcp通讯
  - xmitq   使用的传输队列

- 定义接收通道 `def chl(QM2.QM1)`

  接收通道是被动的，只定义名字就可以。大家注意，接收通道的名字一定要和发送通道名一致，他们是靠名字来匹配。

对于通道的属性，应该考虑的因素主要有：

1） 确定通道的运行方式采用长连接的方式还是触发的方式，通常，对于需要频繁启动的通道，不适宜采用触发的方式。若采用触发方式启动通道，触发类型应为FIRST；

2） 对于发送类型的通道,要考虑通道的断开间隔（DISCINT）、短重试次数（SHORTRTY）、短重试间隔（SHORTTMR）、长重试次数（LONGRTY）、长重试间隔（LONGTMR）、批处理大小（BATCHSZ）的配置。

- 要停止 MQSC，输入： end 

  将显示一些消息，后跟命令提示符。

### 第四步 配置监听器

是对方mq管理器来探测，本地要给对方一个回应，监听器就是起这个作用的。

如果是5.3版本 只能在命令行里运行 `runmqlsr -m QM1 -t tcp -p 1414`

如果是6.0版本 可以runmqsc QM1里运行 `def listener(LSR.QM1)trptype(tcp) port(1414) control(qmgr)`

解释下 trptype 监听类型

port          监听端口

control       监听控制，如果是qmgr则在队列管理器启动的时候监听也自动启动。

 

启动监听器

START LISTENER(LSR.QM1)

 

### 第五步 配置另外一个队列管理器

简单的说一下，和上面的差不多，只不过名字不一样。大家自己尝试下：）

 

重新配置一个MQ可以执行runmqsc  CCBDG < CCBDG.mq

## MQ的API

 

 

 

![img](file:///C:/Users/user/AppData/Local/Temp/msohtmlclip1/01/clip_image003.jpg)

 

 

上图揭示了IBM WebSphere MQ 编程的原理。第一步是让应用程序与队列管理器连接。它通过 MQConnect调用来进行此连接。下一步使用 MQOpen 调用为输出打开一个队列。然后应用程序使用 MQPut 调用将其数据放到队列上。要接收数据，应用程序调用 MQOpen 调用打开输入队列。应用程序使用 MQGet 调用从队列上接收数据。

 

在MQ的13个函数中，MQCONN/MQDISC是最耗CPU的两个函数，其次是MQOPEN和MQCLOSE这两个函数，因此要尽量避免必要地重复使用这几个函数。比如，当您需要从队列中读取多条消息时，正确的编程方法应该如下

 

以C语言为例，一个MQ应用的开发流程如下：

MQCONN()  /*建立与队列管理器的连接*/

MQOPEN() /*打开要进行读写操作的队列*/

MQPUT() /*将消息放入队列*/

MQGET() /*从队列中读取消息*/

MQINQ() /*查询队列的属性*/

MQSET() /*设置队列的属性*/

MQCLOSE() /*在读写等操作进行完之后，将队列关闭*/

MQDISC() /*断开与队列管理器的连接，释放相关的资源*/

 

即：连接/断开队列管理器一次，打开/关闭队列一次，读取消息多次。而不应该反复建立与队列管理器的连接和反复进行队列打开/关闭操作。



# mq命令大全

最近在配置MQ,记下了一些常用的MQ命令,如下:

创建队列管理器
crtmqm –q QMgrName
-q是指创建缺省的队列管理器

删除队列管理器
dltmqm QmgrName

启动队列管理器
strmqm QmgrName
如果是启动默认的队列管理器，可以不带其名字

停止队列管理器
endmqm QmgrName 受控停止

endmqm –i QmgrName 立即停止

endmqm –p QmgrName 强制停止

显示队列管理器
dspmq –m QmgrName

运行MQ命令
runmqsc QmgrName
如果是默认队列管理器，可以不带其名字

往队列中放消息
amqsput QName QmgrName
如果队列是默认队列管理器中的队列，可以不带其队列管理器的名字

从队列中取出消息
amqsget QName QmgrName
如果队列是默认队列管理器中的队列，可以不带其队列管理器的名字

启动通道
runmqchl –c ChlName –m QmgrName

启动侦听
runmqlsr –t TYPE –p PORT –m QMgrName

停止侦听
endmqlsr -m QmgrName

下面是在MQ环境中可以执行的MQ命令(即在runmqsc环境下可以敲的命令)

定义持久信队列
DEFINE QLOCAL（QNAME） DEFPSIST（YES） REPLACE

设定队列管理器的持久信队列
ALTER QMGR DEADQ（QNAME）

定义本地队列
DEFINE QL（QNAME） REPLACE

定义别名队列
DEFINE QALIAS(QALIASNAME) TARGQ(QNAME)

远程队列定义
DEFINE QREMOTE（QRNAME） +
RNAME（AAA） RQMNAME（QMGRNAME） +
XMITQ（QTNAME）

定义模型队列
DEFINE QMODEL（QNAME） DEFTYPE（TEMPDYN）

定义本地传输队列
DEFINE QLOCAL(QTNAME) USAGE(XMITQ) DEFPSIST(YES) +
INITQ（SYSTEM.CHANNEL.INITQ）+
PROCESS(PROCESSNAME) REPLACE

创建进程定义
DEFINE PROCESS（PRONAME） +
DESCR（‘STRING’）+
APPLTYPE（WINDOWSNT）+
APPLICID（’ runmqchl -c SDR_TEST -m QM_ TEST’）
其中APPLTYPE的值可以是：CICS、UNIX、WINDOWS、WINDOWSNT等

创建发送方通道
DEFINE CHANNEL（SDRNAME） CHLTYPE（SDR）+
CONNAME（‘100.100.100.215(1418)’） XMITQ（QTNAME） REPLACE
其中CHLTYPE可以是：SDR、SVR、RCVR、RQSTR、CLNTCONN、SVRCONN、CLUSSDR和CLUSRCVR。

创建接收方通道
DEFINE CHANNEL（SDR_ TEST） CHLTYPE（RCVR） REPLACE

创建服务器连接通道
DEFINE CHANNEL（SVRCONNNAME） CHLTYPE（SVRCONN） REPLACE

显示队列的所有属性
DISPLAY QUEUE（QNAME） [ALL]

显示队列的所选属性
DISPLAY QUEUE（QNAME） DESCR GET PUT
DISPLAY QUEUE（QNAME）MAXDEPTH CURDEPTH

显示队列管理器的所有属性
DISPLAY QMGR [ALL]

显示进程定义
DISPLAY PROCESS（PRONAME）

更改属性
ALTER QMGR DESCR（‘NEW DESCRIPTION’）
ALTER QLOCAL（QNAME） PUT（DISABLED）
ALTER QALIAS（QNAME） TARGQ（TARGQNAME）

删除队列
DELETE QLOCAL（QNAME）
DELETE QREMOTE（QRNAME）

清除队列中的所有消息
CLEAR QLOCAL（QNAME）

以下是一些高级配置的命令:

amqmcert 配置SSL证书

amqmdain 配置windows上的MQ服务

crtmqcvx 转换数据

dmpmqaut 转储对象权限管理

dmpmqlog 转储日志管理

dspmq 显示队列管理器

dspmqaut 显示打开对象的权限

dmpmqcap 显示处理程序容量和处理程序数

dspmqcsv 显示命令服务器状态

dspmqfls 显示文件名

dspmqtrc 跟踪MQ输出(HP-UNIX LINUX Solaris)

dspmqrtn 显示事务的详细信息

endmqcsv 停止队列管理器上的命令服务器

strmqcsv 启动队列管理器上的命令服务器

endmqtrc 停止跟踪

rcdmqimg 向日志写对象的映像

rcmqobj 根据日志中的映像重新创建一个对象

rsvmqtrn 提交或逆序恢复事务
\--------------------------

Solaris下MQ管理 有三种命令集合，可用于管理 WebSphere MQ，分别是控制命令、MQSC 命令和 PCF 命令。
-- 查看队列管理器
bash-2.03$ dspmq
QMNAME(DPHKCOP) STATUS(Running)
QMNAME(remote.qm) STATUS(Ended unexpectedly)
QMNAME(remote) STATUS(Ended immediately)
bash-2.03$ dltmqm remote
WebSphere MQ queue manager 'remote' deleted.
bash-2.03$ dltmqm remote.qm
WebSphere MQ queue manager 'remote.qm' deleted.
bash-2.03$
bash-2.03$ dspmq
QMNAME(DPHKCOP) STATUS(Running)

-- MQ command server
bash-2.03$ dspmqcsv DPHKCOP
WebSphere MQ Command Server Status . . : Running
bash-2.03$

-- 区分大小写
bash-2.03$ dspmqcsv DPHKCOP
WebSphere MQ Command Server Status . . : Running
bash-2.03$ dspmqcsv dphkcop
AMQ8146: WebSphere MQ queue manager not available.
=================
总结：
\1. 在MQ的home下面，有些常用的命令（在solaris下，加到了path下）
常用的有：
dspmq - display message queue
\2. 脚本控制命令 runmqsc QName ( if no specify QName, will use default if exists )
它会提示可用的命令，如输入无效
该命令，自动转换为大写字母（除非用引号），所以定义的object的名称都为大写
对于参数不知道，可先查询显示，【 用(*)代替】再进一步操作
AMQ8426: Valid MQSC commands are:
ALTER
CLEAR
DEFINE
DELETE
DISPLAY
END
PING
REFRESH
RESET
RESOLVE
RESUME
START
STOP
SUSPEND
:
display
AMQ8426: Valid MQSC commands are:
DISPLAY AUTHINFO
DISPLAY CHANNEL
DISPLAY CHSTATUS
DISPLAY CLUSQMGR
DISPLAY PROCESS
DISPLAY NAMELIST
DISPLAY QALIAS
DISPLAY QCLUSTER
DISPLAY QLOCAL
DISPLAY QMGR
DISPLAY QMODEL
DISPLAY QREMOTE
DISPLAY QUEUE
DISPLAY QSTATUS
DISPLAY CONN
DISPLAY SERVICE
DISPLAY LISTENER
DISPLAY SVSTATUS
DISPLAY LSSTATUS
DISPLAY QMSTATUS
:
display queue(*)
13 : display queue(*)
AMQ8409: Display Queue details.
QUEUE(SYSTEM.ADMIN.ACCOUNTING.QUEUE) TYPE(QLOCAL)
AMQ8409: Display Queue details.
QUEUE(SYSTEM.ADMIN.ACTIVITY.QUEUE) TYPE(QLOCAL)
AMQ8409: Display Queue details.
QUEUE(SYSTEM.ADMIN.CHANNEL.EVENT) TYPE(QLOCAL)
AMQ8409: Display Queue details.
display service(*)
display QREMOTE(*)
display QMODEL(*)

\----------------------------------------------
dspmq
runmqsc PROVINCE_QM
?
DISPLAY AUTHINFO
DISPLAY CHANNEL
DISPLAY CHSTATUS
DISPLAY CLUSQMGR
DISPLAY PROCESS
DISPLAY NAMELIST
DISPLAY QALIAS
DISPLAY QCLUSTER
DISPLAY QLOCAL
DISPLAY QMGR
DISPLAY QMODEL
DISPLAY QREMOTE
DISPLAY QUEUE
DISPLAY QSTATUS
DISPLAY CONN
DISPLAY SERVICE
DISPLAY LISTENER
DISPLAY SVSTATUS
DISPLAY LSSTATUS
DISPLAY QMSTATUS

# RedHat安装MQ

1、检查操作系统及内存情况：lsb_release -a;

2   检查java 环境：java -version

3   新建用户空间、用户和组：

//创建用户目录

mkdir /home/mqm

//创建用户组

groupadd mqm

//创建用户

useradd -g mqm -d /home/mqm -m -s /bin/bash mqm

4 创建MQ安装文件夹：

//更改目录权限

chown -R mqm:mqm /home/mqm

//创建MQ安装位置和工作空间

mkdir /opt/mqm (安装目录)

mkdir /var/mqm （数据目录）

mkdir /var/mqm/log （日志目录）

mkdir /var/mqm/errors（出错目录）

chown -R mqm:mqm /opt/mqm

chown -R mqm:mqm /var/mqm

（3）vi mqlicense.sh

在setJRE下

JRE＝目录+/bin/java

PATH=$PATH:/bin:/usr/local/bin:/usr/bin:/usr/sbin:/etc:/opt/mqm/bin:.

export PATH

MQM_HOME=/opt/mqm

export MQM_HOME

CLASSPATH=$MQM_HOME/java/lib/com.ibm.mq.jar:$MQM_HOME/java/lib/com.ibm.mqbind.jar:$MQM_HOME/java/lib/com.ibm.mqjms.jar:$MQM_HOME/java/lib/jms.jar:$MQM_HOME/java/lib/jms.jar:$MQM_HOME/java/lib/jndi.jar:$MQM_HOME/java/lib/jta.jar:$MQM_HOME/java/lib/ldap.jar:$MQM_HOME/java/lib/connector.jar:$MQM_HOME/java/lib/fscontext.jar:$MQM_HOME/java/lib/postcard.jar:$MQM_HOME/java/lib/providerutil.jar:$CLASSPATH

export CLASSPATH

(4)在/home/mqm空间中解压安装包，并执行如下安装：

rpm -ivh MQSeriesRuntime-6.0.1-0.x86_64.rpm

rpm -ivh MQSeriesServer-6.0.1-0.x86_64.rpm

rpm -ivh MQSeriesSDK-6.0.1-0.x86_64.rpm

rpm -ivh MQSeriesSamples-6.0.1-0.x86_64.rpm

rpm -ivh MQSeriesJava-6.0.1-0.x86_64.rpm

rpm -ivh MQSeriesClient-6.0.1-0.x86_64.rpm

 

安装验证：

rpm -qa |grep MQSeries

(5) MQ配置：

创建队列管理器：crtmqm 队列管理器名

启动队列管理器：strmqm 队列管理器名

开户strmqbrk代理：strmqmbrk -m 队列管理器名

进入MQ的控制台：runmqsc 队列管理器名

定义管道： define channel(CH1) chltype(SVRCONN) trptype(TCP) mcauser('mqm')

退出控制台：end

建立队列管理器基本的 Queue:

在MQ的安装目录java/bin下执行：runmqsc GCP_QM < MQJMS_PSQ.mqsc，建立一些基本的queue

建立其它所需的Queue

DEFINE QLOCAL （TEST_MQ_LOCALQ1） REPLACE  DEFPSIST(NO)  MAXDEPTH(1000)//创建本地队列

DEFINE QMODEL （TEST _ME_MODELQ1)  REPTYPE(PERMDYN)   DEFPSIST (NO)  MAXDEPTH(1000) SHARE REPLACE

启动监听：

runmqlsr -m 队列管理器名  -t tcp -p 端口号

查看队列管理器字符集：

dis qmgr

修改字符集：

alter qmgr CCSID(字符集号) ；

修改后停止队列管理器： endmqm –i  队列管理器名 s

重启队列管理器：strmqm 队列管理器名。

\---------------------------

开启查看代理的运行状态：

strmqbrk -m 队列管理器名

dspmqbrk -m 队列管理器名

管理控制台常用命令：

进入管理控制台：runmqsc 队列管理器名

查看通道的信息

DISPLAY CHANNEL （通道名）//通道名为CH1,CH2之类的

查看队列管理器状态及关闭队列管理器：

查看：dspmq

关闭队列管理器：endmqm -i 队列管理器名

删除及创建通道：

在管理控制台中执行

进入管理控制台：runmqsc  队列管理器名

删除通道:DELETE CHANNEL (通道名)

新建通道：define channel (通道名) chltype (SVRCONN)  trptype (TCP)  mcauser('mqm')

显示队列管理器中的所有队列：

dis q(*)

查看指定队列的详细信息：

 dis q (队列名称)// 例如TEST1.Q



# 测试

显示队列管理器

```sh
# dspmq
QMNAME(QM000)                                             STATUS(Ended immediately)
QMNAME(QMRC)                                              STATUS(Running)
```

运行MQ命令

```
# runmqsc QMRC
5724-H72 (C) Copyright IBM Corp. 1994, 2011.  ALL RIGHTS RESERVED.
Starting MQSC for queue manager QMRC.
```

```
?
     1 : ?

AMQ8426: Valid MQSC commands are:

    ALTER
    CLEAR
    DEFINE
    DELETE
    DISPLAY
    END
    PING
    REFRESH
    RESET
    RESOLVE
    RESUME
    SET
    START
    STOP
    SUSPEND
```
查看通道 
```
dis chl(*)             
     2 : dis chl(*)
AMQ8414: Display Channel details.
   CHANNEL(CHANNEL1)                       CHLTYPE(SVRCONN)
AMQ8414: Display Channel details.
   CHANNEL(FLCORP.RC)                      CHLTYPE(RCVR)
AMQ8414: Display Channel details.
   CHANNEL(RC.FLCORP)                      CHLTYPE(SDR)
AMQ8414: Display Channel details.
   CHANNEL(RC.RTCORP)                      CHLTYPE(SDR)
AMQ8414: Display Channel details.
   CHANNEL(RTCORP.RC)                      CHLTYPE(RCVR)
AMQ8414: Display Channel details.
   CHANNEL(SYSTEM.AUTO.RECEIVER)           CHLTYPE(RCVR)
AMQ8414: Display Channel details.
   CHANNEL(SYSTEM.AUTO.SVRCONN)            CHLTYPE(SVRCONN)
AMQ8414: Display Channel details.
   CHANNEL(SYSTEM.DEF.CLUSRCVR)            CHLTYPE(CLUSRCVR)
AMQ8414: Display Channel details.
   CHANNEL(SYSTEM.DEF.CLUSSDR)             CHLTYPE(CLUSSDR)
AMQ8414: Display Channel details.
   CHANNEL(SYSTEM.DEF.RECEIVER)            CHLTYPE(RCVR)
AMQ8414: Display Channel details.
   CHANNEL(SYSTEM.DEF.REQUESTER)           CHLTYPE(RQSTR)
AMQ8414: Display Channel details.
   CHANNEL(SYSTEM.DEF.SENDER)              CHLTYPE(SDR)
AMQ8414: Display Channel details.
   CHANNEL(SYSTEM.DEF.SERVER)              CHLTYPE(SVR)
AMQ8414: Display Channel details.
   CHANNEL(SYSTEM.DEF.SVRCONN)             CHLTYPE(SVRCONN)
AMQ8414: Display Channel details.
   CHANNEL(SYSTEM.DEF.CLNTCONN)            CHLTYPE(CLNTCONN)
```
查看通道 
```
dis chl(FLCORP.RC)
     9 : dis chl(FLCORP.RC)
AMQ8414: Display Channel details.
   CHANNEL(FLCORP.RC)                      CHLTYPE(RCVR)
   ALTDATE(2017-10-09)                     ALTTIME(22.44.24)
   BATCHSZ(10)                             COMPHDR(NONE)
   COMPMSG(NONE)                           DESCR( )
   HBINT(300)                              KAINT(AUTO)
   MAXMSGL(10485760)                       MCAUSER( )
   MONCHL(QMGR)                            MRDATA( )
   MREXIT( )                               MRRTY(10)
   MRTMR(1000)                             MSGDATA( )
   MSGEXIT( )                              NPMSPEED(FAST)
   PUTAUT(DEF)                             RCVDATA( )
   RCVEXIT( )                              RESETSEQ(NO)
   SCYDATA( )                              SCYEXIT( )
   SENDDATA( )                             SENDEXIT( )
   SEQWRAP(999999999)                      SSLCAUTH(REQUIRED)
   SSLCIPH( )                              SSLPEER( )
   STATCHL(QMGR)                           TRPTYPE(TCP)
   USEDLQ(YES)      
```
查看通道状态
```
dis chs(FLCORP.RC)
    10 : dis chs(FLCORP.RC)
AMQ8417: Display Channel Status details.
   CHANNEL(FLCORP.RC)                      CHLTYPE(RCVR)
   CONNAME(168.33.131.13)                  CURRENT
   RQMNAME(QMCORP)                         STATUS(RUNNING)
   SUBSTATE(RECEIVE)                    
```
>`RUNNING`为正常

查看远程队列

```
dis qr(*)
    12 : dis qr(*)
AMQ8409: Display Queue details.
   QUEUE(BATCORP_2)                        TYPE(QREMOTE)
AMQ8409: Display Queue details.
   QUEUE(CORP_2)                           TYPE(QREMOTE)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.DEFAULT.REMOTE.QUEUE)      TYPE(QREMOTE)
```
查看本地队列 
```
dis ql(*)
    14 : dis ql(*)
AMQ8409: Display Queue details.
   QUEUE(BATRC_1)                          TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(DEADQ)                            TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(ERRMSG)                           TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(FL_CORP)                          TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(RC_1)                             TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(RT_CORP)                          TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.ACCOUNTING.QUEUE)    TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.ACTIVITY.QUEUE)      TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.CHANNEL.EVENT)       TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.COMMAND.EVENT)       TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.COMMAND.QUEUE)       TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.CONFIG.EVENT)        TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.LOGGER.EVENT)        TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.PERFM.EVENT)         TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.PUBSUB.EVENT)        TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.QMGR.EVENT)          TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.STATISTICS.QUEUE)    TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.TRACE.ACTIVITY.QUEUE)
   TYPE(QLOCAL)                         
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.ADMIN.TRACE.ROUTE.QUEUE)   TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.AUTH.DATA.QUEUE)           TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.BROKER.ADMIN.STREAM)       TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.BROKER.CONTROL.QUEUE)      TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.BROKER.DEFAULT.STREAM)     TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.BROKER.INTER.BROKER.COMMUNICATIONS)
   TYPE(QLOCAL)                         
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.CHANNEL.INITQ)             TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.CHANNEL.SYNCQ)             TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.CHLAUTH.DATA.QUEUE)        TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.CICS.INITIATION.QUEUE)     TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.CLUSTER.COMMAND.QUEUE)     TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.CLUSTER.HISTORY.QUEUE)     TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.CLUSTER.REPOSITORY.QUEUE)
   TYPE(QLOCAL)                         
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.CLUSTER.TRANSMIT.QUEUE)    TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.DEAD.LETTER.QUEUE)         TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.DEFAULT.INITIATION.QUEUE)
   TYPE(QLOCAL)                         
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.DEFAULT.LOCAL.QUEUE)       TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.DOTNET.XARECOVERY.QUEUE)   TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.DURABLE.SUBSCRIBER.QUEUE)
   TYPE(QLOCAL)                         
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.HIERARCHY.STATE)           TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.INTER.QMGR.CONTROL)        TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.INTER.QMGR.FANREQ)         TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.INTER.QMGR.PUBS)           TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.INTERNAL.REPLY.QUEUE)      TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.PENDING.DATA.QUEUE)        TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.PROTECTION.ERROR.QUEUE)    TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.PROTECTION.POLICY.QUEUE)   TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.RETAINED.PUB.QUEUE)        TYPE(QLOCAL)
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.SELECTION.EVALUATION.QUEUE)
   TYPE(QLOCAL)                         
AMQ8409: Display Queue details.
   QUEUE(SYSTEM.SELECTION.VALIDATION.QUEUE)
   TYPE(QLOCAL)  
```
> 其中301、302、317业务对应的队列一般为`BATRC_1`或`JJDZ_*`



查看本地队列 

```
dis ql(BATRC_1)
    15 : dis ql(BATRC_1)
AMQ8409: Display Queue details.
   QUEUE(BATRC_1)                          TYPE(QLOCAL)
   ACCTQ(QMGR)                             ALTDATE(2017-10-09)
   ALTTIME(22.43.33)                       BOQNAME( )
   BOTHRESH(0)                             CLUSNL( )
   CLUSTER( )                              CLCHNAME( )
   CLWLPRTY(0)                             CLWLRANK(0)
   CLWLUSEQ(QMGR)                          CRDATE(2017-10-09)
   CRTIME(22.43.33)                        CURDEPTH(0)
   CUSTOM( )                               DEFBIND(OPEN)
   DEFPRTY(0)                              DEFPSIST(YES)
   DEFPRESP(SYNC)                          DEFREADA(NO)
   DEFSOPT(SHARED)                         DEFTYPE(PREDEFINED)
   DESCR( )                                DISTL(NO)
   GET(ENABLED)                            HARDENBO
   INITQ( )                                IPPROCS(0)
   MAXDEPTH(200000)                        MAXMSGL(10485760)
   MONQ(QMGR)                              MSGDLVSQ(PRIORITY)
   NOTRIGGER                               NPMCLASS(NORMAL)
   OPPROCS(0)                              PROCESS( )
   PUT(ENABLED)                            PROPCTL(COMPAT)
   QDEPTHHI(80)                            QDEPTHLO(20)
   QDPHIEV(DISABLED)                       QDPLOEV(DISABLED)
   QDPMAXEV(ENABLED)                       QSVCIEV(NONE)
   QSVCINT(999999999)                      RETINTVL(999999999)
   SCOPE(QMGR)                             SHARE
   STATQ(QMGR)                             TRIGDATA( )
   TRIGDPTH(1)                             TRIGMPRI(0)
   TRIGTYPE(FIRST)                         USAGE(NORMAL)
```

> 其中`CURDEPTH(0)`为队列的深度，也可理解为是否有数据发送过来。

```
?
    16 : ?

AMQ8426: Valid MQSC commands are:

    ALTER
    CLEAR
    DEFINE
    DELETE
    DISPLAY
    END
    PING
    REFRESH
    RESET
    RESOLVE
    RESUME
    SET
    START
    STOP
    SUSPEND
```

- `START ` / `STOP`
- `RESET`（保证发送端和接收端计数器一致）



# 常见问题

> 消息无法发送？

- 查看队列的状态是否有异常；
- `RESET`保证发送端和接收端计数器一致。