---
layout: post
title: 2021 GreenTeaJUG 活动 上海 精彩回顾
date: 2021-07-06
tags: ["2021","article","活动","活动ppt"]
---

<!-- wp:paragraph -->

2021年6月19日下午，GreenTea JUG时隔两年在上海举办了线下Meetup活动。国内外知名公司资深Java领域工程师相聚在浦东国际人才港，与众多参会人员面对面分享了最新Java技术的探索和实践。本次GreenTea JUG由阿里云开发者社区、OpenAnolis社区联合举办。

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

GreenTea JUG（[http://greenteajug.cn/](http://greenteajug.cn/)）是中国最大的Java用户组（Java User Group），也是中国第一个JCP（Java Community Process）成员。GreenTea JUG旨在传播最新的Java前沿技术与开发方式。

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

本次Meetup由四个讲座构成，涵盖了Alibaba Dragonwell的ZGC技术、Intel在WebAssembly的实践、阿里在异构体系下的Java性能分析和编程语言领域资深专家对Java语言的思考。本次活动场面火爆，除了精彩的讲座本身，讲师与参会人员交流的过程也迸出激烈的思维火花。

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

**Production Ready ZGC for Java11：Alibaba Dragonwell11 上规模化实践**

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

首个议题的讲师是来自阿里云JVM团队的唐浩。他首先介绍了垃圾回收器GC暂停对于Java业务SLA的影响，对比了Java的几种主流GC，然后介绍了Java11的新一代垃圾回收器ZGC的业务价值和理论背景，开启ZGC可以解决SLA响应时间的P99，P999等百分位的长尾问题。随后，讲师展示了ZGC服务于阿里业务和云上客户的落地成果以及不足之处。这些不足之处包括重大的C2 load barrier的缺陷引发的程序crash，以及类卸载、物理内存归还、各种非暂停因素的RT延长、避免OOM等功能缺失。讲师展示了阿里JVM团队应对这些不足的方案，主要包括移植上游成熟的ZGC代码，也包括新增一部分可调节的选项。阿里的方案既能解决落地时遇到的问题，也采用宏隔离等方式确保了Dragonwell11的代码质量。讲师最后还展望了未来ZGC工作的方向，并介绍了阿里JVM团队的开源项目JIFA。

<!-- /wp:paragraph -->

[下载](http://greenteajug.github.io/images/ZGC_in_dragonwell.pdf)
<!-- /wp:file -->

<!-- wp:paragraph -->

**WebAssembly Micro Runtime 多线程管理**

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

第二个议题的讲师是来自Intel的徐君。他首先介绍了WebAssembly和WebAssembly Micro Runtime(WAMR)项目，后者为前者提供了高效、实用、轻量、可靠的运行时。讲座主要内容包括WAMR线程管理架构及实现和WAMR pthread适配层。讲师最后展示了WASM在Tensorflow-lite上的多线程实验结果。

<!-- /wp:paragraph -->

[下载](http://greenteajug.github.io/images/WAMR-thread-management.pptx)

<!-- wp:paragraph -->

**异构体系下的Java应用性能分析**

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

第三个议题的讲师是来自阿里集团技术风险与效能部的技术专家潘宇峰。他首先展示了Alibaba Dragonwell性能爆发式提升的成果，然后分享了新硬件平台的全栈性能优化，以及异构体系下性能对比分析的通用方法，该方法是可复现的，能够流程化、自动化。

<!-- /wp:paragraph -->

[下载](http://greenteajug.github.io/images/异构体系下的Java性能分析.pdf)

<!-- wp:paragraph -->

**2021年还在学Java**

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

第四个议题的讲师是来自字节跳动的陆传胜。他是一位OpenJDK committer。在讲座过程中，他分享了关于编程语言技术的思考，从编程语言的诞生聊到在企业中的大规模。讲座语言生动幽默，却又发人深省。

<!-- /wp:paragraph -->

[下载](http://greenteajug.github.io/images/2021_still_learning_Java_.pdf)