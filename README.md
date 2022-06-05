# sleep(easy)

任务要求:

创建一个sleep的用户态程序，sleep应该暂停用户指定的ticks，ticks是由xv6内核定义的一个概念，即来自计时器芯片两次中断之间的时间。您的解决方案应该在文件user/sleep.c中。

~~~shell
$./test.sh
init: starting sh
$sleep 10
(运行一会儿什么也不会发生)
~~~



1. 可以参考user/echo.c,user/grep.c,user/grep.c,user/rm.c,了解如何传递给程序的命令行参数。
2. 如果用户忘记传递参数，sleep应该打印一条错误消息。
3. 字符串可以通过atoi转化成为整数。
4. 使用系统调用sleep
5. 在Makefile文件里添加你的sleep到UPROGS，完成后./test.sh可以运行xv6。

# pingpong(easy)

任务要求:

编写一个程序，使用unix系统调用一对管道在两个进程之间"pingpong"一个字节，一个管道用于每个方向。parent应该向child发送一个字节，子进程应该打印"<pid>:receive ping",其中<pid>是它的进程id，将管道上的字节写入父进程，然后退出；parent应该child那读取字节，打印"<pid>:receive pong"然后退出。您的解决方案应该在文件user/pingpong.c中。

1. 使用pipe创建管道

2. 使用fork创建一个child

3. 使用read从pipe读取，并用write写入管道

4. 使用getpid查找调用进程的进程id

5. 将程序添加到makefile中的UPROGS

6. xv6上的用户程序有一组有限的函数库可供他们使用。您可以在user/user.h中看到列表，还有一些位于user/lib.c,user/printf.c和user/umalloc.c中。

~~~shell
$./test.sh
init: starting sh
$pingpong
4: received ping
3: received pong
$
~~~

# primes(moderate)/(hard)   

任务要求:

使用管道编写一个并发版本的筛。[方法](https://swtch.com/~rsc/thread/)中详细写明了做法。您的解决方案应该在文件user/primes.c中。

您的目标是使用pipe和fork来设置管道。第一个过程将数字2~35输入管道。对于每个素数，您将安排创建一个进程，该进程通过管道从其左邻居读取并通过另一管道向右邻居写入。由于xv6的文件描述符和进程数量有限，第一个进程可以在35处停止。

1. 小心关闭进程不需要的文件描述符，不然有可能在第一个进程达到35的时候xv6资源不足
2. 一旦第一个进程达到35，他应该等到整个终止
3. 将32位int写入管道必格式化的ASCII i/O写入管道要简单
4. 将程序添加到makefile中的UPROGS

~~~shell
$ ./test.sh
...
init: starting sh
$ primes
prime 2
prime 3
prime 5
prime 7
prime 11
prime 13
prime 17
prime 19
prime 23
prime 29
prime 31
$
~~~

# find(moderate)

任务要求:

编写一个简单版本的unix查找程序：查找目录树中具有特定名称的所有文件。您的解决方案应该在文件`user/find.c`中。

1. 查看 user/ls.c 以了解如何读取目录。
2. 使用递归允许 find 下降到子目录。
3. 不要递归到“.” 和 ”..”。
4. 每次运行test.sh会自动重置对文件系统的更改。
5. 请注意， == 不会像 Python 中那样比较字符串。请改用 strcmp()。
6. 将程序添加到Makefile 中的`UPROGS`。

~~~
$ ./test.sh
...
init: starting sh
$ echo > b
$ mkdir a
$ echo > a/b
$ find . b
./b
./a/b
$ 
~~~
