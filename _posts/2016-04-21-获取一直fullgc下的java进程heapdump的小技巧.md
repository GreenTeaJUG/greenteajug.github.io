---
layout: post
title: 获取一直FullGC下的java进程HeapDump的小技巧
date: 2016-04-21
tags: ["2016","@JianhaoMo","@善良的右席","article"]
---

作者：[@善良的右席](http://weibo.com/u/2059648327)

### 小技巧

我们应用的java进程出问题的时候，我们往往会用jmap或者gcore拿到一份HeapDump，拿到MAT上做一次Heap分析，`但是` 如果你排查的是一直在FullGC的gc问题，你Dump下来的堆往往是正处于FullGC中，可能会导致`分析失败`。@坤谷 发现了一个小技巧，可以dump下完整而且没有在被移动中的Heap。

### 操作流程

*   找到java进程，gdb attach上去， 例如 `gdb java -p 22443`
*   找到这个`HeapDumpBeforeFullGC`的地址（这个flag如果为true，会在FullGC之前做HeapDump，默认是false）

    (gdb) p &HeapDumpBeforeFullGC
    $2 = (<data variable, no debug info> *) <span class="hljs-number">0x7f7d50fc660f</span> <HeapDumpBeforeFullGC>

*   然后把他设置为true，这样下次FGC之前就会生成一份core文件

    (<span class="hljs-name">gdb</span>) set *0x7f7d50fc660f = <span class="hljs-number">1</span>
    (<span class="hljs-name">gdb</span>) quit

*   最后，等一会，等下次FullGC触发，你就有HeapDump了！
(PS. `jstat -gcutil pid` 可以查看gc的概况)

&nbsp;
<div id="comment-89354" class="comment media gaze in">
<div class="media-body">
<div class="voters-p">注意事项，设完，观察heapdump生成路径和磁盘，别生成太多，让磁盘满了，heapdump出来后，及时kill掉应用。</div>
</div>
</div>
<div id="comment-89355" class="comment media gaze in"></div>
&nbsp;

Q&A
Q1：把HeapDumpAfterFullGC也设上是不是会更快点儿？
A1：那就是被fullgc完之后的堆了，会不会堆里有些业务垃圾会有助于分析嘛。。

Q2: HeapDumpBeforeFullGC 可以用JCMD改的吧， 我看是manageable 属性
A2: 是的，如果jcmd，jinfo能改，就先用工具。这里是所有别的工具都失败的情况下的办法。一直fullgc的时候jinfo会不生效吧

Q3: jmx，mxbean能直接动态改的，比如去Jconsole里setVmOption，直接将HeapDumpBeforeFullGC和HeapDumpAfterFullGC设置成true，也比较方便的A3: 嗯嗯是的，但是这类方法缺点就是如果应用一直无限fullgc，会导致修改不成功

Q4: 一直fgc，用jmap -histo，我觉得就基本上能看出蛛丝马迹了
A4: 不错。其实很多时候还不需要heapdump，taobao-jdk6/ali-jdk8/ajdk8的大数组警告，直接能看出问题。

Q5: jmap -F 不行吗
A5: 那个时候dump下来的堆，可能正处于gc中，会解析失败

Q6: 文章中不是说等下次full gc吗？既然等到下一次full gc，我觉得jmap -F能dump出文件吧？或者直接设置jvm -XX:+UseHeapDumpBeforeFullGC应该也可以吧？不知道说的对不对
A6: jmap -F的原理和gcore差不多，强行dump出内存，这个时候如果正在gc中，那么内存中对象的引用关系可能是乱的。XX:+UseHeapDumpBeforeFullGC这个flag，线上应用肯定是不会开的。上面的方法就是强制打开这个flag