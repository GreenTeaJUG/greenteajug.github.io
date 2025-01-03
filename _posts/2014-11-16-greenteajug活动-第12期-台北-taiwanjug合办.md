---
layout: post
title: GreenTeaJUG活动 第12期 台北 TaiwanJUG合办
date: 2014-11-16
tags: ["2014","@bluedavy_readonly","@ferhui","@JianhaoMo","台北","活动"]
---

时间：2014-11-16
地点：台北
组织：[@JianhaoMo](http://weibo.com/halmo)

GreenTeaJUG参与TaiwanJUG的主题

主题：阿里JVM之路
阿里是一个重度使用Java的公司，这样就驱使着公司走在实践JVM的路上。我们主要关注三点：性能优化、故障排查与回馈社区。性能优化方面，针对特定应 用需求在JVM内增加了一些intrinsic方法，例如CRC32、CRC32C、byte/char数组比较，ASCII与UTF8相互转换等,对于 GC,我们给出了一个创新的Off-Heap方案；在故障排查方面，增加ArrayAllocationWarningSize、 PrintGCReason、ReclaimMostNativeMemory等参数，辅助排查问题；在回馈社区方面，我们发现GC、socket连接泄 漏等bug，问题解决后，将patch回馈到了社区。
讲师：费辉[@ferhui](http://weibo.com/u/2651541140)，资深开发工程师，就职于阿里巴巴集团-阿里云事业部-核心系统研发部，2004~2008 本科就读于中国科学技术大学；2008~2011 硕士就读于中国科学院软件研究所，研究方向为并行计算；2011年7月加入阿里巴巴-核心系统研发部-专用计算组，从事JVM优化相关工作，针对公司应用的特点，定制化JVM。例如结合JVM，对hadoop namenode中rpc进行优化等等。近期从事系统级profiling的工作，包括但不限于java应用，目标低开销、稳定地运行在服务器上，能够给出开发者函数级的优化建议，异或是当应用出问题的时候，能够提供有力的性能数据帮助问题排查。项目网站：[http://jvm.taobao.org](http://jvm.taobao.org)

主题：Java常见问题排查方法
每个软件在运行时不可避免的会出现各种各样的故障，严重的故障会使得用户对网站的信任度下降，甚至产生严重的社会影响和经济损失，故障的排查和解决过程就像是一场争分夺秒的战争，排查的技巧和经验在这个时候特别的重要。淘宝网是上千个应用组成的网站，主要由Java编写而成，出现过的故障种类非常的多，从而积累了不少排查问题的方法。这次分享涵盖以下常见的几种Java问题的排查方法：(1). 类加载问题；(2). 内存OOM问题；(3). CPU消耗高问题；(4). Java Crash问题；(5). 分布式调用超时问题
讲师：林昊（[bluedavy_readonly](http://weibo.com/u/1072396800)），林昊因故没能出席，由费辉代讲ppt。林昊  网络ID: bluedavy，资深技术专家，目前就职于阿里巴巴集团-技术保障部。2007年底加入淘宝，2008-2010年负责淘宝服务框架的设计与实现，此服务框架是淘宝3.0架构体系中的重要组件，在淘宝大范围使用，2011年每天承载的服务请求量为300亿+；2009年与同事共同出版《OSGi原理与最佳实践》一书，2010年出版《分布式Java应用：基础与实践》一书；2011年负责HBase在淘宝的落地，目前HBase在阿里的各家公司使用广泛，为海量数据读写的业务提供了支持；2011年下半年至今主导T4产品（基于LXC），目标为大幅度的降低淘宝的运维成本；2013年转为运维，着力关注运维领域，主要涉及的为可维护性、稳定性、性能、成本、软硬件结合，并根据发展需要推动系统结构演变。个人网站：[http://hellojava.info](http://hellojava.info)