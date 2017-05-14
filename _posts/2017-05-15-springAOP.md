---
layout: post
title:  "spring之AOP"
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
 

- 代理模式

![image](http://7xpuj1.com1.z0.glb.clouddn.com/%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F.png)

---

- ISubject:该接口是对被访问者或者被访问资源的抽象。
- SubjectImpl:被访问者或者被访问资源的具体实现。
- SubjectProxy:被访问者或者被访问资源的代理实现类，该类持有一个ISubject接口的具体实例。


SubjectImpl 和SubjectProxy都实现了相同的接口ISubject，而SubjectProxy内部持有SubjectImpl的引用。当Client通过request()请求服务的时候，SubjectProxy将转发给SubjectImpl。
