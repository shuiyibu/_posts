按照shell语法，后一个前台命令必须等待前一个前台命令执行完毕才能进行，这就是所谓的单线程程序。如果两条命令之间有依赖性还好，否则后一条命令就白白浪费了等待的时间了。

网上查了一遍，shell并没有真正意义上的多进程。而最简单的节省时间，达到“**多线程**”效果的办法，是将前台命令变成后台进程，这样一来就可以跳过前台命令的限制了。

引用网上例子：

实例一：全前台进程：

```
#!/bin/bash  
#filename:simple.sh  
starttime=$(date +%s)  
for ((i=0;i<5;i++));do  
        {  
                sleep 3;echo 1>>aa && endtime=$(date +%s) && echo "我是$i,运行了3秒,整个脚本执行了$(expr $endtime - $starttime)秒"  
        }  
done  
cat aa|wc -l  
rm aa  
```





运行测试：

\# time sh simple.sh

我是0,运行了3秒,整个脚本执行了3秒

我是1,运行了3秒,整个脚本执行了6秒

我是2,运行了3秒,整个脚本执行了9秒

我是3,运行了3秒,整个脚本执行了12秒

我是4,运行了3秒,整个脚本执行了15秒

12

real    0m15.131s

user    0m0.046s

sys     0m0.079s

”我是X,运行了3秒“就规规矩矩的顺序输出了。



实例二：使用'&'+wait 实现“多进程”实现

```
#!/bin/bash  
#filename:multithreading.sh  
starttime=$(date +%s)  
for ((i=0;i<5;i++));do  
        {  
                sleep 3;echo 1>>aa && endtime=$(date +%s) && echo "我是$i,运行了3秒,整个脚本执行了$(expr $endtime - $starttime)秒"  
        }&  
done  
wait  
cat aa|wc -l  
rm aa  
```

运行测试：

\# time sh  multithreading.sh

我是2,运行了3秒,整个脚本执行了3秒

我是3,运行了3秒,整个脚本执行了3秒

我是1,运行了3秒,整个脚本执行了3秒

我是4,运行了3秒,整个脚本执行了3秒

我是0,运行了3秒,整个脚本执行了3秒

5

real    0m3.105s

user    0m0.065s

sys     0m0.115s

运行很快，而且很不老实（顺序都乱了,大概是因为expr运算所花时间不同）

解析：这一个脚本的变化是在命令后面增加了&标记，意思是将进程扔到后台。在shell中，后台命令之间是不区分先来后到关系的。所以各后台子进程会抢夺资源进行运算。

wait命令：

wait  [n]

n 表示当前shell中某个执行的后台命令的pid，wait命令会等待该后台进程执行完毕才允许下一个shell语句执行；如果没指定则代表当前shell后台执行的语句，wait会等待到所有的后台程序执行完毕为止。

如果没有wait，后面的shell语句是不会等待后台进程的，一些对前面后台进程有依赖关系的命令执行就不正确了。例如wc命令就会提示aa不存在。

实例三：可控制后台进程数量的“多进程”shell

   前一个shell也虽然达到“多进程”执行效果，但是并发执行的子进程数量却是不受控制的。

   想想看，如果某一个作业需要执行1万次，那么到底是单进程实现还是像实例1那样多进程实现呢？前者，你慢慢等待吧；后者，祝你好运，希望你的计算机同时执行那么多进程不宕机，不卡死。

##    而实例3则可以实现可控制进程数量的**多线程**。

```sh
#!/bin/sh  
function a_sub {  
        sleep 3;  
        echo 1>>aa && endtime=$(date +%s) && echo "我是$i,运行了3秒,整个脚本执行  
了$(expr $endtime - $starttime)秒" && echo  
}  
starttime=$(date +%s)  
export starttime  
tmp_fifofile="/tmp/$.fifo"  
echo $tmp_fifofile  
mkfifo $tmp_fifofile  
exec 6<>$tmp_fifofile  
rm $tmp_fifofile  
thread=3  
for ((i=0;i<$thread;i++));  
do  
echo  
done >&6  
for ((i=0;i<10;i++))  
do  
read -u6  
{  
a_sub || {echo "a_sub is failed"}  
echo >&6  
} &  
done  
wait  
exec 6>&-  
wc -l aa  
rm -f aa  
exit 0  
```



