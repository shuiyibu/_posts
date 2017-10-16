#  MQ中间件报错AMQ8146的解决方法

执行命令runmqsc时，报错“AMQ8146: MQSeries queue manager not available.”，如下：

```
$ runmqsc QMgrName
*(C)Copyright 1998-2002 Willow Technology, Inc. and its Licensors.  *
ALL RIGHTS RESERVED.
Starting MQSeries Commands.
AMQ8146: MQSeries queue manager not available.
No MQSC commands read.
No commands have a syntax error.
All valid MQSC commands were processed.
```



原因：队列管理器未启动。

解决办法：启动mq队列管理器QMgrName。

启动命令：`$ strmqm  QMgrName`