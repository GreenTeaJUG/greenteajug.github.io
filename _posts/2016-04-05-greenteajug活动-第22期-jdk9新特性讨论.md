---
layout: post
title: GreenTeaJUG活动 第22期 JDK9新特性讨论
date: 2016-04-05
tags: ["2016","@JianhaoMo","活动","网络"]
---

时间：2016-04-05
地点：网络
组织：[@JianhaoMo](http://weibo.com/halmo)

甲骨文希望听到中国Java社区关于JDK9的反馈，欢迎任何形式的反馈。

> The attachment is the JDK9 new features introduction, could you help to forward it to China Java User Groups? JDK9 first introduces Jigsaw feature, it causes big changes in JDK structure and APIs, we would contact China Java User Group to collect their questions. Could you help to copy me when you send this presentation to the user groups?

￼￼￼￼￼￼> ￼￼￼￼￼￼This Is Not A Drill
> Prepare For JDK 9
> 
> Rory O'Donnell
> Senior Quality Manager OpenJDK Quality Lead
> Java Platform Group @ Oracle March 19th, 2016
> ￼￼￼￼￼
> ￼Prepare for JDK 9
> ￼￼￼￼
> ￼Categories of JDK-internal APIs
> http://openjdk.java.net/jeps/260
> • Non-critical
> - No evidence of use outside of JDK 
> - or used only for convenience
> • Critical
> - Functionality that would be difficult, if not impossible, to implement outside of the JDK
> ￼￼￼￼
> ￼JEP 260 Proposal
> http://openjdk.java.net/jeps/260
> • Encapsulate all non-critical internal APIs by default
> • Encapsulate all critical internal APIs for which supported replacements exist in JDK 8
> • Do not encapsulate critical internal APIs 
> - Deprecate them in JDK 9
> - Plan to remove in JDK 10
> - Provide a workaround via command-line flag
> ￼￼￼￼
> ￼JEP 260 Proposal
> http://openjdk.java.net/jeps/260
> • Propose as critical internal APIs - sun.misc.Unsafe
> - sun.misc.{Signal,SignalHandler}
> - sun.misc.Cleaner
> - sun.reflect.Reflection::getCallerClass 
> - sun.reflect.ReflecWonFactory
> ￼￼￼￼
> ￼Finding uses of JDK-internal APIs
> http://openjdk.java.net/jeps/260
> • jdeps tool in JDK 8, improved in JDK 9 • Maven JDeps Plugin
> ￼￼￼￼
> ￼Removed 6 deprecated methods
> http://openjdk.java.net/jeps/162
> • Flagged for removal in JSR 337, and JEP 162
> • Removed
> - java.util.logging.LogManager::addPropertyChangeListener
> - java.util.logging.LogManager::removePropertyChangeListener 
> - java.util.jar.Pack200.Packer::addPropertyChangeListener
> - java.util.jar.Pack200.Packer::removePropertyChangeListener
> - java.util.jar.Pack200.Unpacker::addPropertyChangeListener
> - java.util.jar.Pack200.Unpacker::removePropertyChangeListener
> ￼￼￼￼
> ￼Change the binary structure of the JRE and JDK
> http://openjdk.java.net/jeps/220
> • Motivation
> • Not an API but still a disruptive change
> • Details in JEP 220
> • In JDK 9 since late 2014 to give lots of time for the tools to catch up
> ￼￼￼￼
> ￼Removed
> http://openjdk.java.net/jeps/220
> • Endorsed standards override mechanism 
> • Extension mechanism
> ￼￼￼￼
> ￼Other changes
> http://openjdk.java.net/jeps/261
> • Application and extension class loaders are no longer instances of java.net.URLClassLoader
> • Removed: -Xbootclasspath and -Xbootclasspath/p are removed
> • Removed: system property sun.boot.class.path
> • JEP 261 has the full list of the issues that we know about
> ￼￼￼￼
> ￼New version-string scheme
> http://openjdk.java.net/jeps/223
> • Old versioning format is difficult to understand 
> • New format addresses these problems
> • Impacts java -version and related properties
> ￼￼￼￼
> ￼￼What can you do to prepare?
> https://wiki.openjdk.java.net/display/AdopKon/JDK+9+Outreach
> • Check code for usages of JDK-internal APIs with jdeps
> • If you develop tools then check code for a dependency on rt.jar or tools.jar or the runtime-image layout
> • Check code that might be sensitive to the version change
> • Check code for uses of underscore as an identifier
> • Check code for uses of unrecognized VM opWons such as -XX:MaxPermSize
> • Test the JDK 9 EA builds and Project Jigsaw EA builds
> ￼￼￼￼
> ￼Thank you!
> Quality Outreach effort wiki:
> https://wiki.openjdk.java.net/display/quality/Quality+Outreach

￼￼￼￼
￼￼￼￼￼￼
￼￼￼￼￼￼