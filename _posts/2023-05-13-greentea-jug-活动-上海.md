---
layout: post
title: 2023 GreenTeaJUG 活动 上海
date: 2023-05-11
tags: ["2023","article","活动"]
---
# GreenTea JUG 2023-5-13活动回顾

2023-05-16 施懿民 GreenTeaJUG

疫情褪去，春暖花开，Green Tea JUG的线下活动重启了，来自阿里、Azul、华为、字节的技术大咖们在这美好的季节为我们带来了JVM领域的前沿动态，干货满满！

感谢Green Tea JUG发起人莫简豪的议题组织
感谢华为、Azul和阿里提供的干货话题
感谢西门子创新中心提供的场地
感谢微软Reactor提供的直播设备和支持
感谢知平和见田创服小伙伴们的现场组织

## OpenJDK RISK-V Port概述

华为 - 朱艳红

第一个议题是由来自华为毕昇JDK团队，OpenJDK RISC-V Port的开发者之一朱艳红。本次分享内容主要是RISC-V 指令集架构的部分特性，以及基于这些特性，OpenJDK 主线上RISC-V Port中的解决方案。 

议题大纲：

1. No Flag Register
2. Zifencei: FENCE.I

3. Vector Extension

4. Bitmanipulation Extension

5. JIT Extension: Pointer Masking

相关资料

1. https://github.com/openjdk/riscv-port

2. Huawei BishengJDK
    https://gitee.com/openeuler/bishengjdk-8
    https://gitee.com/openeuler/bishengjdk-11
    https://gitee.com/openeuler/bishengjdk-17 

3. OpenJDK RISC-V Binary

    https://builds.shipilev.net/openjdk-jdk/ 

----

## Cloud Native Compiler概述

Azul - Gavin Yu


第二个议题是来自全球最大的OpenJDK商业版提供商Azul Systems的Gavin为我们分享云原生编译器的理念。

云原生编译器（Cloud Native Compiler）主要由两个服务组成，帮助Java进程更快启动和更快执行。


编译服务：提供了一个服务器端优化解决方案，可以将JIT编译转移到单独的专用服务资源，为JIT编译提供更多的处理能力，同时将客户端JVM从本地执行JIT编译的负担中解放出来。

性能剖析日志服务：记录和服务ReadyNow剖析文件。这大大简化了CNC编译器服务的操作使用，不需要为写入剖析文件配置本地存储。Cloud Native Compiler可以记录来自多个JVM的多个候选性能剖析文件，并推广最优记录的剖析文件。

参考资料：

    https://docs.azul.com/cloud_native_compiler/ 

    https://github.com/zulu-openjdk 

    https://foojay.io/ 

----

## CRaC简介

Azul - Gerrit

第三个议题同样是来自Azul Systems的Gerrit Grunwald为我们分享JVM中的CRaC（Coordinate Restore at Checkpoint）。

在微服务越来越成为云上运行的基于Java的应用程序的标准架构的世界里，JVM的预热时间可能会受到限制。尤其是当你考虑启动应用程序的新实例以响应负载变化时，预热时间可能会成为一个问题。本地镜像是解决这些问题的一种解决方案，因为它们的静态提前编译代码不需要预热，因此启动时间很短。但即使启动时间更短、占用内存更小，它也并非没有缺点。由于运行时缺少JIT优化，总体性能可能会较慢。有一个新的OpenJDK项目称为CRaC（Coordinated Restore at Checkpoint 检查点协调恢复），其目标是用不同的方法解决JVM预热问题。其想法是抓取运行中的JVM的快照，将其存储在文件中，并在稍后的时间点（甚至在另一台机器上）恢复JVM。

本次分享将向您简要介绍CRaC项目，并展示概念验证实现的一些结果。

参考资料：

    https://openjdk.org/projects/crac/

    https://github.com/CRaC

    https://github.com/HanSolo/crac8 


----

## Java机密计算

阿里 - 林子熠

最后一个话题是来自阿里巴巴JVM团队技术专家林子熠带来的Java机密计算。林子熠博士同时还是《GraalVM与Java静态编译原理与应用》的作者。今天这个话题是在JUG首秀，下周才会公开发表出来。

Java机密计算的动机：

机密计算通过硬件隔离加密的方式保护应用中的安全敏感数据和程序算法在运行时的安全性，在多方计算，同态加密，区块链，联邦计算等领域有重要应用。但是机密计算并不支持Java程序，Teaclave Java SDK开发实现了支持Java机密计算的框架和SDK，已在Apache开源，与上海交通大学合作的论文也获得了软工国际顶会ICSE的杰出论文奖。

----

报名： [[https://www.huodongxing.com/event/6521652048700](https://www.huodongxing.com/event/3700655142300)]

![_config.yml](http://cdn.huodongxing.com/file/ue/20150311/11505A5F7FE767FD334DBE663A51AA9CAB/30454865664427924.jpg)
