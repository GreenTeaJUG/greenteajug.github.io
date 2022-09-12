---
layout: post
title: GreenTeaJUG活动 第16期 性能调优利器——PerfJ by 周敏
date: 2015-06-28
tags: ["2015","@MinZhou","以亭","活动","网络"]
---

时间：2015-06-28
地点：网络
组织：以亭

主题：Perfj
讲师：周敏（[@MinZhou](http://weibo.com/u/1309690680)）

[![周敏](周敏-300x200.jpg)](http://greenteajug.cn/wp-content/uploads/2015/06/周敏.jpg)

周敏, 暨南大学计算机硕士毕业学位. 主研方向: 大数据、分布式系统. 先后在阿里巴巴担任技术专家, 美国LinkedIn公司Staff Engineer , 现任美国Tango公司Senior Staff Engineer, 带领大数据基础研发团队. 多个Apache开源项目贡献者及提交者. 曾帮助Apache Spark团队获100TB数据排序世界记录. 现定居美国旧金山湾区.

Java作为服务端程序近年来大规模网站服务、大数据、机器学习等方面应用越来越广, 服务端程序的性能越来越关键。当前Java的性能剖析器,例如jvisualvm, jprofiler, YJP都只能从Java代码层面分析Java程序的性能, 无法从JVM, 系统甚至硬件层面找出性能的关键点. 而Linux系统层的perf, systemtap等工具能够查到磁盘IO, 网络IO, CPU, 内存等性能问题，但无法上达至Java层. 中间的这个断层使我们分析Java程序性能非常不便. 最近由演讲者开发的PerfJ可以让开发者从下自上地分析Java程序的性能. 演讲者将阐述perfj的原理, 举例怎么用perfj找到由于CPU cache miss引发的性能问题, 以及分析系统context switch造成的性能问题.

活动形式：微信群讨论（报名成功后获取入群二维码）

活动报名请按照如下格式发邮件到event@greenteajug.cn

姓名：XXX
电话号码：xxxxxxxxxxx
邮箱：xxxx@xxx.xxx
公司：xxxx有限公司
职位：xx工程师
Java使用年限：x年