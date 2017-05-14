---
layout: post
title:  "spring学习之AOP"
date:   2017-04-23 00:06:05
categories: 技术
tags: spring
excerpt: spring之AOP
---


* content
{:toc}

---


### spring 之AOP

####  概述

采用动态代理机制和字节码生成技术，动态代理机制和字节码生成都是在运行期间为目标对象生成一个代理对象，而将横切逻辑织入到代理对象，系统最终使用的是织入了横切逻辑的代理对象，不是真正的目标对象。


######  AOP词汇

- Joinpoint
  
  在系统运行之前，AOP的功能模块都需要织入到OOP的功能模块中。所以，要进行这种织入过程，我们需要知道在系统的那些执行点上进行织入操作，这些将要在其之上进行织入操作的系统执行点就称之为Joinpoint.

  基本上，程序执行过程的任何时点都可以作为横切逻辑的织入点，这些都是Joinpoint.
  
  **常见Jpinpoint**:
  
  方法调用：当方法被调用的时候所处的程序执行点。
  
  方法调用执行：某个方法内部执行开始时点，应该与方法调用类型的Joinpoint进行区分。
  
  方法调用是在调用对象上的执行点，而方法执行则是在被调用到的方法逻辑执行的时点。对于同一对象，方法调用先于方法执行。
  
  构造方法调用，构造方法执行，字段设置，字段获取，异常处理执行，
  
- Pointcut

  Pointcut概念代表的是Joinpoint的表述方式。将横切逻辑织入当前系统的过程中，需要参照Pointcut规定的Joinpoint信息，才可以知道应该在系统的那些Joinponit上织入横切逻辑。
  
  Pointcut指定了系统中符合条件的一组Joinpoint。
  
  **ponitcut的表述方式**：
    - 直接指定Joinpoint所在方法名称：只限于支持方法级别的Joinpoint的AOP框架。
    - 正则表达式：利用表达式，来归纳表述需要符合某种条件的多组Joinpoint。
    - 使用特定的Ponitcut表述语言：AspectJ使用这种方式来指定Pointcut,它提供了一种类似正则表达式的针对Pointcut的表述语言
 

###### 代理模式

![image](http://7xpuj1.com1.z0.glb.clouddn.com/%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F.png)

---

- ISubject:该接口是对被访问者或者被访问资源的抽象。
- SubjectImpl:被访问者或者被访问资源的具体实现。
- SubjectProxy:被访问者或者被访问资源的代理实现类，该类持有一个ISubject接口的具体实例。


SubjectImpl 和SubjectProxy都实现了相同的接口ISubject，而SubjectProxy内部持有SubjectImpl的引用。当Client通过request()请求服务的时候，SubjectProxy将转发给SubjectImpl。
