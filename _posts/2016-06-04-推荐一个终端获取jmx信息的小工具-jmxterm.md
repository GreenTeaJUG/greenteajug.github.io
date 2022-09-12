---
layout: post
title: 推荐一个终端获取jmx信息的小工具——jmxterm
date: 2016-06-04
tags: ["@JianhaoMo","文章"]
---

作者[@JianhaoMo](http://weibo.com/halmo)

最近由于某些原因跟同学推荐了下jmxterm，发现还是不少同学不知道有这么个工具，所以在这里推荐下。

官网：http://wiki.cyclopsgroup.org/jmxterm/

jmxterm是本地命令行看jmx的工具，jdk7以上自带的jcmd命令可以动态打开关闭本地jmx

举例看codecache:

jcmd <pid> ManagementAgent.start_local

echo "get -d java.lang -b name=Code\ Cache,type=MemoryPool Usage" '  java -jar jmxterm-1.0-alpha-4-uber.jar -l <pid> -v silent -n

jcmd <pid> ManagementAgent.stop