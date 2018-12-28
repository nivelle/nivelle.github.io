---
layout: post
title:  "学习计划"
date:   2018-07-24 01:06:05
categories: 技术
tags: 学习计划
excerpt: 学习大纲
---


* content
{:toc}


### 底层基础

#### Netty
#### JVM
#### Tomcat

### 运维

#### docker
#### Maven
- 生命周期和插件
- 使用Archtype生成项目骨架
- 自定义Archtype
- Nexus搭建私服
- 插件编号

#### git

- git命令最佳实践
- git工作流程剖析
- 分支切换和checkout
- 分支合并
- 分支合并和冲突解决
- 交互式rebase

### 源码

#### Mybatis

[源码解析](https://blog.csdn.net/nmgrd/article/details/54608702)
1. 代码生成器
2. Mybatis一级缓存实现原理以及性能分析
3. Mybatis二级缓存实现原理运行过程
4. Mybatis 插件开发原理

#### Spring5 

[源码解析](https://zhuanlan.zhihu.com/p/28577417)
1. IOC容器（IOC容器和依赖反转，BeanFactory和ApplicationContext,BeanDefinition,FactoryBean）
2. AOP（aopProxy代理对象，Spring AOP拦截器调用，SpringAOP设计与应用场景）
3. 事务处理（声明式事务原理与过程，事务的传播属性，事务实现方式，Spring与JDBC事务，Spring与JTA事务）
4. MVC（原理介绍，Handler Mapping详解，HandlerAdapter详解，ViewResolver详解，SpringMVC详解，模版赢取技术，动态页面的静态化处理）

#### Spring boot(自动配置，CLI,Actuator,热部署，核心注解)
[springboot](http://fangjian0423.github.io/categories/springboot/)


### Tomcat调优

- web程序容器结构剖析
- tomcat内部原理与线程模型
- tomcat系统参数介绍与分析

### mysql调优

- 数据库存储引擎
- mysql索引以及优化
- sql语句性能分析
- sql性能优化

### JVM调优

- jvm原理剖析
- jvm内存大小设置（每个线程栈大小，设置JVM最大堆内存，设置年轻代大小，设置持久代大小）
- 垃圾回收器选择（窜行，并行（吞吐量有限），并发（响应实践优先））
- 参数调优

### 高并发分布式技术

- 分布式事务（传统强一致性ACID，2p/3p/tcc解析协议，分布式架构理论基础-cap/base理论），最终一致性
- Nginx（反向代理，负载均衡，保护隐私数据，nginx和https,Nginx动静分离实践，Nginx缓存集成实战，lua脚本实现业务拓展，高可用）
- zk(分布式，真分布式集群，核心数据模型，watcher,ACL,Paxo协议全解析，zab,分布式锁，集群选举，源码解读)
- dubbo (dubbo框架原理解剖，dubbo集群容错与负载均衡，消息中间件整合，源码分析)
- 分布式缓存（redis数据结构与对象，持久化策略RDB与AOF,主从复制原理，哨兵机制，高可用redis集群架构，redis事务与管道，订阅与发布机制，性能优化与注意事项，穿透，击穿，雪崩）
- 分布式消息队列（JMS,AMQP；rabbitMQ高可用性能集群搭建，rabbit核心知识要领，一切为了解耦，restAPI,Kafka高性能集群搭建，kafka日志消息处理原理，kafka实现生产者消费者模型实践，Kafka应用日志采集以及统计分析实战）

### 分布式数据存储

- sharding-JDBC及核心概念，分布分布，编排治理，分布式事务解析，主键策略，分页以及子查询优化，应用性能监控，基于Keepalive的高可用实战
- springcloud体系
- hbase
- OpenResty
- docker

---

### java

#### 集合

- Collection(list,Set,Queue)
- Map(SortedMap,HashMap,TreeMap,hashTable)
- Iterator

#### 线程

- 概念，好处
- 启动和终止线程（启动线程，中断，如何安全终止线程）
- 线程状态，优先级，demon线程
- 常用方法解析（sleep.wait,notify）
- 线程间的协作/通信（volidate和synchronized）,(等待和通知机制)，（管道输入输出流），join,ThreadLocal
- 线程应用实例（连接池，线程池）

#### JMM内存模型

- 实现原理（volidate实现原理，synchronized的实现原理，原子操作的实现原理）
- java内存模型（内存模型，重排序，内存语义）
- 原子操作类（CAS,原子操作的实现原理）
- 原子操作类使用（AtomicBoolean,AtomicInteger,AtomicIntegerArray，AtomicLong,AtomicLongArray,AtomicReference,AtomicRefrenceArray,AtomicReferenceFieldUppdater）

#### juc

- 容器（ConcurrentHashMap）,ConcurrentSkipListMap,CopyOnWriteArrayList
- 阻塞队列
- 并发工具类（CountDownLatch,CyclicBarrier,Semaphore,Exchange）

#### 线程池和Executor框架

- 线程池ThreadPoolExecutor
- Executor(组成部分，Executors,FutureTask,CompletionService)

### 数据结构和算法


