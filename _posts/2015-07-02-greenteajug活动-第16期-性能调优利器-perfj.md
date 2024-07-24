---
layout: post
title: 性能调优利器——PerfJ
date: 2015-07-02
tags: ["2015","@MinZhou","以亭","article"]
---

**讲师：周敏（[@MinZhou](http://weibo.com/u/1309690680)）**

**整理：以亭**

## 1. Principle behind PerfJ

PerfJ是我业余时间开发的一个小工具，现在来说不建议用在生产环境，因为还没有得到更多案例的证明。

平时我们在做Java程序Profiling时，基本上使用YJP, hprof, jvisualvm这样的工具，还有for i in {1..100}; do jstack $pid > $i; done；这些工具基本上两个原理，一个是sampling(采样)，一个是instrument(注入一些信息到方法里面，然后统计)。

这些工具有些问题不能解决：

*   如果是网络io，磁盘io等问题，基本只能看到epollWait, socket/file的read这一层，看不到更深层次的原因；
*   如果看到Java程序占用CPU很高，用上面工具只能估计。先用top看到线程级别，看哪个线程CPU搞，然后再jstack。
即系统工具都无法上达Java这一层。

PerfJ的原理

linux perf 是一个强大的profiling, tracing的工具集, 包含在kernel代码树里，但perf对于java的程序没有办法。

首先, java很多程序被Just in time compiling了, perf不知道java函数的原始名字。

其次，JVM由于历史原因把frame pointer的register给去了，所以无法爬栈。

PerfJ其实只是linux perf的一个包装。解决了第一个问题，JIT，原理是通过JVMTI把perfj attach到目标JVM，将它JIT后各函数的原始函数名和地址的对照拿出来，写到/tmp/perf-$pid.map文件中，之后perf会加载这个文件，从而统计出java程序的一些信息。第二个问题可以通过升级JDK到8u60 b19以上解决（见后面）。

## 2. Install perf, PerfJ, java 8u60 b19

安装前准备：

Linux 64bit 物理机；

&nbsp;

(1)安装perf工具

centos系：yum install perf.x86_64；

ubuntu：apt-get install linux-tools-common linux-tools-generic linux-tools-`uname -r`

(2)安装PerfJ

checkout the source from github

git clone https://github.com/coderplay/PerfJ.git

and run below in the future buildingcd PerfJ

./gradlew releaseTarGz

如果没有装gcc，可以到http://blog.minzhou.info/PerfJ/PerfJ-1.0.tgz这里下载，但不保证都能用 :)

(3)下载火焰图生成工具FlameGraph

git clone https://github.com/brendangregg/FlameGraph.git

*   安装最新JDK
[https://jdk8.java.net/download.html](https://jdk8.java.net/download.html)

务必是jdk 8u60 b19以上！

如果打不开，可以到这里下载：[http://share.weiyun.com/cd36ccd7f6e837bd4699f3a1448f31e8](http://share.weiyun.com/cd36ccd7f6e837bd4699f3a1448f31e8)

下载后解压，修改PATH和JAVA_HOME环境变量。

export JAVA_HOME=新jdk路径

export PATH=$JAVA_HOME/bin:$PATH

## 3. Show some flame graph

[http://blog.minzhou.info/perfj/perfj.svg](http://blog.minzhou.info/perfj/perfj.svg)

![_config.yml](http://greenteajug.github.io/images/1.png)

图中，绿色是Java代码，黄色是JVM代码，红色是Kernel代码，宽度表示占用CPU时间。

## 4. Building PerfJ and untar the ball

解压perfj-1.0.tgz

cd perfj-1.0/

运行：bin/perfj list， 查看所支持的事件。运行截图：

![_config.yml](http://greenteajug.github.io/images/2.png)

## 5. Context Switch

下载模拟context switch的java代码：[http://blog/minzhou.info/perfj/ContextSwitchTest.java](http://blog/minzhou.info/perfj/ContextSwitchTest.java)

编译：javac -cp . ContextSwitchTest.java

运行：java -cp . ContextSwitchTest

开启另一窗口，运行vmstat 1，发现cs很高。

使用java -cp . -XX:+PreserveFramePointer ContextSwitchTest，开启frame pointer

同时另一窗口运行：bin/perfj record -e cs  -g -p `pgrep -f ContextSwitchTest`

跑一段时间后，ctrl + C，然后bin/perfj report -stdio

得到结果如下：

![_config.yml](http://greenteajug.github.io/images/3.png)

如果出现这种无法打开map文件错误：

![_config.yml](http://greenteajug.github.io/images/4.png)

可能是perfj版本问题。下载这个：[http://blog.minzhou.info/perfj/perfj-1.0.tgz](http://blog.minzhou.info/perfj/perfj-1.0.tgz)（打不开？用[http://share.weiyun.com/ae0cb80416b3ca3d97fc1877aa1bde4a](http://share.weiyun.com/ae0cb80416b3ca3d97fc1877aa1bde4a)）就好了。

从这个程序的stack可以看出Unsafe.park会导致当前线程让出cpu, 引发context switch。

刚才那段代码是用来看context switch在哪里发生，以前的工具是没有办法的。

## 6. CPU cache miss example

下载代码：blog.minzhou.info/PerfJ/L1CacheMiss.java

编译：javac -cp . L1CacheMiss.java

运行：java -cp . -Xmx2g -XX:+PreserveFramePointer L1CacheMiss

捕获：bin/PerfJ record -e  L1-dcache-load-misses  -g -p `pgrep -f CacheMiss`

报告：bin/perfj report -stdio

cache hit比cache miss快几十倍。

![_config.yml](http://greenteajug.github.io/images/5.png)

通过上面看到，94%的cache miss是cachemiss这个函数造成的。

## 7. Homework , Threadpool comparison

时间关系，未做介绍

## 8. More complex examples, io request profiling

下载可执行程序：http://blog.minzhou.info/PerfJ/leveldb-benchmark.jar

直接运行： java -cp leveldb-benchmark.jar  -XX:+PreserveFramePointer org.iq80.leveldb.benchmark.DbBenchmark  --benchmarks=fillrandom  --num=100000000

捕获：bin/perfj record -F 99 -g -p `pgrep -f DbBenchmark`

-F是指sampling的频率为99Hz。

报告：bin/perfj report -stdio

## 9. Off cpu analysis

在cpu上运行的有时候不占程序最主要时间，有时候往往是网络在等，有时候是写了一个sleep代码，这个往往很难找。我们继续用刚才的leveldb 例子：

运行： java -cp leveldb-benchmark.jar -XX:+PreserveFramePointer org.iq80.leveldb.benchmark.DbBenchmark --benchmarks=fillrandom --num=100000000

捕获：bin/PerfJ record -e sched:sched_stat_sleep -e sched:sched_switch  -e sched:sched_process_exit -g -o ~/perf.data.raw -p `pgrep -f Benchmark`

因为我们可以知道系统scheduler的trace point，所以我们record三个事件，然后通过这三件事件的时间差，知道哪个是off CPU之后不干活。

注入：bin/PerfJ inject -v -s -i ~/perf.data.raw -o ~/perf.data

报告： bin/PerfJ report --stdio --show-total-period -i ~/perf.data

![_config.yml](http://greenteajug.github.io/images/6.png)

可以看到截屏的这些stack虽然不占CPU，但在off cpu的时候一直歇着，很有可能拖慢程序的执行。

我们可以看到LockSupport.park的jni实现里有JVM sleep，所以在抢java concurrent锁的时候，如果冲突很大，还可能被锁sleep一下了。大家可以线下在网络代码里试试off cpu。

这里有很多示例：

[http://www.brendangregg.com/FlameGraphs/cpuflamegraphs.html](http://www.brendangregg.com/FlameGraphs/cpuflamegraphs.html)

[http://www.brendangregg.com/perf.html](http://www.brendangregg.com/perf.html)

大部分是perf的用法，大家也可以用perfj试试，大部分都可以用。

例如上面leveldb的例子我们可以分析IO：

bin/perfj record -e block:block_rq_issue -F 99 -g -p `pgrep -f DbBenchmark`

用这个看物理设备的块请求是谁操作的，是哪些java代码调用的，然后我可以看到这个stack：

![_config.yml](http://greenteajug.github.io/images/7.png)

知道了java代码位置，可以用ftrace看这个io request的io size，用了多少时间，是random io还是seq io。

[https://github.com/brendangregg/perf-tools/blob/master/examples/iosnoop_example.txt](https://github.com/brendangregg/perf-tools/blob/master/examples/iosnoop_example.txt)

# ./iosnoop Tracing block I/O... Ctrl-C to end.

COMM             PID    TYPE DEV      BLOCK        BYTES     LATmssupervise        1809   W    202,1    17039968     4096       1.32supervise        1809   W    202,1    17039976     4096       1.30tar              14794  RM   202,1    8457608      4096       7.53tar              14794  RM   202,1    8470336      4096      14.90tar              14794  RM   202,1    8470368      4096       0.27tar              14794  RM   202,1    8470784      4096       7.74tar              14794  RM   202,1    8470360      4096       0.25tar              14794  RM   202,1    8469968      4096       0.24tar              14794  RM   202,1    8470240      4096       0.24tar              14794  RM   202,1    8470392      4096       0.23tar              14794  RM   202,1    8470544      4096       5.96tar              14794  RM   202,1    8470552      4096       0.27tar              14794  RM   202,1    8470384      4096       0.24[...]

&nbsp;

像这样，perfj找到系统的事件之后，然后用ftrace跟踪详细的信息。

## 10. CPU flame graph

捕获成功后，会在当前目录下生成perf.data。利用这个文件可以绘制火焰图。

sudo mv perf.data ../FlameGraph

cd ../FlameGraph/

sudo perf script ' ./stackcollapse-perf.pl > out.perf-folded

./flamegraph.pl out.perf-folded --colors java > PerfJ.svg