---
layout: post
title: GreenTeaJUG活动 第11期 杭州
date: 2014-09-23
tags: ["2014","@JianhaoMo","@善良的右席","千臂","曼努","杭州","活动"]
---

时间：2014-09-23,2014-09-24
地点：杭州文一西路969号阿里巴巴西溪园区1-2-7 曼陀山庄
组织：[@JianhaoMo](http://weibo.com/halmo)

主题：HotCode2，Java热部署技术
讲师：千臂

主题：Truffle and Graal: Building fast interpreters on top of the JVM
I will present how to build interpreters using the Truffle framework. Existing virtual machines usually have the JIT compiler deeply integrated into the virtual machine and the run-time. The Java Truffle framework and Graal compiler, however, separate this two concerns: A language implementer writes an Abstract Syntax Tree (AST) interpreter using Truffle. Upon execution, the framework will let Graal compile its hot execution paths by assuming the structure of the AST interpreter and performing related optimizations. The result is fast code, portability over platforms, and language interoperability.
讲师：曼努

主题：延续(continuation)，及其在编译器优化中的应用
内容大致包括；
1.continuaiton与CPS(continuation passing style)的介绍。
2.函数式语言编译器的常用中间表示(IR)。
3.简单示例：如何在解释器中使用CPS实现尾递归优化(TCO)。
4.扩展：如何应对更加复杂的情况。
讲师：[@善良的右席](http://weibo.com/u/2059648327)