---
layout: post
title: fastjson 1.2.10版本发布，修复Bug，支持Class Level SerializeFilter
date: 2016-04-23
tags: ["2016","@温高铁","article"]
---

作者：温绍锦（[@温高铁](http://weibo.com/wengaotie)）阿里巴巴资深技术专家

# Bug Fixed

1.  修复ValueFilter导致序列化数据丢失的问题，这个在1.2.9优化序列化引起。
2.  修复某些场景下解析json中的注释出错。[issue 559](https://github.com/alibaba/fastjson/issues/559)
3.  修复WriteNonStringValueAsString特性打开时，非public类序列化会导致int类型输出为0的问题。 [issue 572](https://github.com/alibaba/fastjson/issues/572)
4.  修复1.2.8/1.2.9版本不支持JDK 1.5的问题

# 功能增强

1.  新增Class Level SerializeFilter支持，在此之前只能在toJSONString时SerializeFilter，对所有的类型都起作用，这样会对框架的实现由性能影响，新特性允许SerializeFilter注册在类型上，具体文档看这里[https://github.com/alibaba/fastjson/wiki/Class_Level_SerializeFilter](https://github.com/alibaba/fastjson/wiki/Class_Level_SerializeFilter)

# 相关链接

*   下载 [http://repo1.maven.org/maven2/com/alibaba/fastjson/1.2.10/](http://repo1.maven.org/maven2/com/alibaba/fastjson/1.2.10/)
*   文档 [https://github.com/alibaba/fastjson/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98](https://github.com/alibaba/fastjson/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
*   源码 [https://github.com/alibaba/fastjson/tree/1.2.10](https://github.com/alibaba/fastjson/tree/1.2.10)