本文介绍了shell脚本传递变量到另一个脚本文件中的方法，在脚本中调用另一脚本，即创建了一个子进程，感兴趣的朋友参考下。
一，有如下的shell脚本。
father.sh



复制代码 代码示例:

```sh
#!/bin/bash
echo "this is the father"
FILM="A Few Good Men"
 
echo "I like the film : $FILM"
 
#call the child script
#export FILM
./child.sh
 
echo "back to father"
echo "and the film is : $FILM"
 
exit
```


二，子shell脚本代码：child.sh



复制代码 代码示例:

```sh
#!/bin/bash
 
echo "called from father...i am the child"
echo "filem name is : $FILM"
 
FILM="Die Hard"
echo "changing film to :$FILM"
 
exit
```



结果如下：




解析：这是因为 father 中并没有导出变量 FILM 给 child。


当 father 把 变量 FILM 导出给 child，child脚本就知道了 FILM变量的值了，结果如下：




因为 father 把变量 FILM用 export命令导出了，所以任意的脚本都可以使用 变量 FILM 了，它们将继承的 FILM的所有权。


注意：不可以将子进程的变量导出到父进程；要实现这一点，可以通过重定向。