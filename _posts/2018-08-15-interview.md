---
layout: post
title:  "java必知必会"
date:   2018-08-09 00:06:05
categories: 技术
tags: interview
excerpt: interview
---

* content
{:toc}



#### 使用mysql索引都有哪些原则？索引有什么数据结构？ B+tree 和 B tree 什么区别

- ([**mysql索引章节**](http://nivelle.me/2017/05/04/索引/))

- ([mysql树结构](http://blog.jobbole.com/111680/))

- ([mysql索引实现](https://www.cnblogs.com/zlcxbb/p/5757245.html))

- ([索引使用](https://juejin.im/post/5b14e0fd6fb9a01e8c5fc663))
  
#### mysql 有哪些存储引擎？有什么区别？

- ([mysql索引类型比较](https://www.cnblogs.com/changna1314/p/6878900.html)) 

- ([mysql索引类型以及特点](https://my.oschina.net/junn/blog/183341))

- ([mysqlAUTO_INCREMENT](https://blog.csdn.net/iamczb/article/details/43112689))

#### 设计高并发系统数据库层面应该怎么设计？ 数据库锁有哪些类型？如何实现

- ([高并发系统设计-数据层](https://blog.csdn.net/chenpeng19910926/article/details/51789934))

- ([mysqlAUTO_INCREMENT](https://blog.csdn.net/wangkehuai/article/details/46727203))

- ([**mysql高并发方案**](https://blog.csdn.net/kidoo1012/article/details/54691561))

- ([架构设计 数据库方案](https://www.w3cschool.cn/architectroad/architectroad-arv823bf.html))


#### 数据库事物有哪些？

- ([**数据库事务基础**](http://nivelle.me/2017/06/05/%E4%BA%8B%E7%89%A9%E5%AD%A6%E4%B9%A0(%E4%B8%80)%E4%B9%8B%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5/))

- ([**数据库事务使用**](http://nivelle.me/2017/06/10/%E4%BA%8B%E5%8A%A1%E4%BD%BF%E7%94%A8/))

#### 如何设计可以动态扩容的分库分表方案？ 以及底层原理？常见的分库分表中间件？优缺点？ 如何让未分库分表的数据动态切换到分库分表的系统上？分库分表解决主键问题?

- ([分库分表方案](https://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=2650994413&idx=1&sn=24a01089ee47793b5d82381b04a34499&chksm=bdbf0ebe8ac887a8a75a0cd9226bb7e0427f1a77c43323d14dc6932c7dc77555531bce5dce0f&scene=0))

----

#### 分布式事物？如何实现？TCC？ 网络出现问题，如何容错？

- ([分布式事务](http://www.infoq.com/cn/articles/solution-of-distributed-system-transaction-consistency#))

- ([分布式事务阶段提交](http://int64.me/2016/%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%8B%E5%8A%A12PC%20&&%203PC.html))

- ([TCC](https://juejin.im/post/5a74f3bc6fb9a0633f0df127))

- ([分布式事务实现](https://blog.csdn.net/tzs_1041218129/article/details/80086991))

#### 分布式寻址方式方式有哪些算法？ 一致性hash算法

- ([**一致性hash算法**](http://www.zsythink.net/archives/1182))

- ([**递归算法**](https://blog.csdn.net/justloveyou_/article/details/71787149))

#### redis和 memcheched什么区别？为什么单线程的redis比多线程的memched效率高？

- ([redis的特点和原理](https://juejin.im/post/5ad6e4066fb9a028d82c4b66))

#### redis主要数据类型？分别那种场景下使用？

 - ([动态字符串](http://nivelle.me/2017/07/24/redis%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0(%E4%B8%80)/))

- ([链表](http://nivelle.me/2017/07/24/redis%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0(%E4%BA%8C)%E4%B9%8B%E9%93%BE%E8%A1%A8/))

- ([跳跃表](http://nivelle.me/2017/09/07/redis%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0(%E4%B8%89)%E4%B9%8B%E8%B7%B3%E8%B7%83%E8%A1%A8/))

- ([字典](http://nivelle.me/2017/09/04/redis%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0(%E5%9B%9B)%E4%B9%8B%E5%AD%97%E5%85%B8/)) 


#### redis的主从复制怎么实现的？redis集群模式是如何实现的？ redis的key是如何寻址的？

- ([redis面试总结](http://www.cnblogs.com/DarrenChan/p/8796922.html))

- ([主从复制](https://my.oschina.net/zhaolin/blog/752520))

#### 使用redis如何设计分布式锁？ 使用zk可以吗？如何实现？两种效率更高**？

- ([分布式锁](http://www.hollischuang.com/archives/1716))

#### redis基本知识

- ([**redis基本知识**](https://github.com/CyC2018/CS-Notes/blob/master/notes/Redis.md#%E5%8D%81%E4%B8%89%E6%95%B0%E6%8D%AE%E6%B7%98%E6%B1%B0%E7%AD%96%E7%95%A5))

#### zk原理? zk的应用？ paxos算法？

- ([**zk原理**](http://nivelle.me/2016/04/12/Zookeeper%E5%AD%A6%E4%B9%A0-%E4%BA%8C-%E4%B9%8Bzk%E5%9F%BA%E7%A1%80/))

- ([**zk应用**](http://nivelle.me/2017/04/12/Zookeeper%E5%AD%A6%E4%B9%A0-%E4%B8%89-%E4%B9%8Bzk%E7%9A%84%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF/))

- ([**paxos算法**](https://www.cnblogs.com/linbingdong/p/6253479.html))

- ([zk概述](https://www.cnblogs.com/felixzh/p/5869212.html))

#### dubbo的实现过程？注册中心挂了可以继续通信么？dubbo常见配置有哪些

- ([dubbo常见特性](https://www.cnblogs.com/aspirant/p/9003255.html))

#### dubbo支持哪些序列化协议？hession?hession数据结构？ pb知道么？为啥PB的效率是最高的？

- ([dubbo学习](https://www.cnblogs.com/aspirant/p/9002631.html))

- ([xstream、protobuf、protostuff](https://www.cnblogs.com/xiaoMzjm/p/4555209.html))

#### dubbo负载均衡策略和高可用策略有哪些？动态代理策略呢?为什么要进行系统拆分啊？ 拆分不用dubbo可以么？ dubbo和thirft什么区别？

- ([**rpc框架**](http://www.cnblogs.com/aspnet2008/p/?page=1))

- ([**dubbo的负载均衡**](https://www.cnblogs.com/aspirant/p/8994227.html))

#### 不用应用环境下的会话保持

- ([会话保持](http://blog.51cto.com/virtualadc/592454))

#### 自己实现RPC

- ([**Dubbo**](http://www.cnblogs.com/LBSer/p/4853234.html))

- ([**轻量级分布式 RPC 框架**](https://my.oschina.net/huangyong/blog/361751))

- ([**简单rpc**](http://www.cnblogs.com/codingexperience/p/5930752.html))

----

#### netty 可以干什么？ NIO，BIO ，AIO 都是什么？ 有什么区别

- ([**NIO,BIO,AIO**](https://www.cnblogs.com/aspirant/p/6877350.html))


#### 为什么使用消息队列？消息队列的优点和缺点？

- （[消息队列的意义](https://www.w3cschool.cn/architectroad/architectroad-message-queue.html)）

#### 如何保证消息队列的高可用？如何保证消息不被重复消费？

- ([**消息队列的幂等**](https://www.w3cschool.cn/architectroad/architectroad-message-idempotence.html))

- ([**消息队列的高可用**](https://www.w3cschool.cn/architectroad/architectroad-message-delivery.html))

#### kafka，activemq,rabbitmq,rocketmq 都有什么优点和缺点？如何自己设计一个消息队列，该如何进行架构设计

- ([消息队列大全](https://www.cnblogs.com/aspirant/category/1195858.html))

- ([消息队列细节学习](https://blog.csdn.net/u013256816))

- ([kafka数据一致性和zk的比较](https://www.cnblogs.com/aspirant/p/9179045.html))

#### rabbitMQ基础

- ([**rabbitMQ**](https://www.kancloud.cn/longxuan/rabbitmq-arron/117513))

#### 如何设计一个高并发高可用系统？

- ([高并发](https://www.w3cschool.cn/architectroad/architectroad-high-concurrent.html))

- ([高可用](https://www.w3cschool.cn/architectroad/architectroad-high-availability.html))

#### 如何限流？工程中怎么做的？说下具体实现？

- ([限流](https://www.w3cschool.cn/architectroad/architectroad-optimization-of-seckilling-system.html))

#### 负载均衡

- ([六大负载均衡原理](https://www.cnblogs.com/aspirant/p/9087716.html))

- ([lvs](https://www.cnblogs.com/aspirant/p/9084740.html))

#### 缓存如何使用？缓存使用不当带来什么问题**？

- ([缓存界的三大问题](https://juejin.im/post/5aa8d3d9f265da2392360a37))

#### 如何降级？如何进行系统拆分，如何进行数据库拆分？

- ([服务降级](http://jinnianshilongnian.iteye.com/blog/2306477))

#### http的工作流程？越细节越好。  http1.0 1.1 1.2有哪些区别

- ([一次url请求](https://github.com/skyline75489/what-happens-when-zh_CN))

#### tcp 三次握手，四层分手的工作流程〉为什么不是其他次数？

- ([计算机网络面试常见](https://www.jianshu.com/p/d0dd47afabad))

#### 海量数据如何去重？

- ([海量数据去重](https://blog.csdn.net/paul_wei2008/article/details/21170999))

- ([大数据排序](https://blog.csdn.net/michellechouu/article/details/27230451))

#### hashcode相等两个类一定相等吗？ equals 呢？ 相反呢？

- ([**hashCode()和equals()**](https://www.cnblogs.com/skywang12345/p/3324958.html))

#### 介绍下集合框架

- ([容器](https://blog.csdn.net/u013256816/article/details/50925091))

#### 常见数据结构

- ([常见数据结构](https://blog.csdn.net/fighterandknight/article/category/6193841))

- ([常见数据结构2](https://blog.csdn.net/zxt0601/article/category/6697194))

- ([moudCount的作用](https://blog.csdn.net/qq_24235325/article/details/52450331))

- ([CopyOnWriteArrayList](https://blog.csdn.net/linsongbin1/article/details/54581787))

- ([hashtable和hashMap](https://blog.csdn.net/tgxblue/article/details/8479147))

- ([**hashMap非线程安全分析**](http://www.importnew.com/22011.html))

- ([hashMap key==null 分析](https://blog.csdn.net/glory1234work2115/article/details/50825503))

- ([**String**](https://blog.csdn.net/justloveyou_/article/details/52556427))

- ([hashMap分析](http://note.youdao.com/noteshare?id=e5a60b1100b28bb62c7b9a101efe477c))

- ([hashMap分析2](https://blog.csdn.net/tuke_tuke/article/details/51588156))

- ([ConcurrentHashMap分析](http://www.importnew.com/28263.html))


#### 线程池用过么？都有哪些参数？ 底层如何实现？

- ([多线程面试](https://www.cnblogs.com/aspirant/category/1017858.html))

- ([**任务的抽象**](https://blog.csdn.net/justloveyou_/article/details/79846241))

- ([**线程池**](https://blog.csdn.net/fngy123/article/details/48738409))

- ([AQS](http://www.idouba.net/sync-implementation-by-aqs/))

- ([**线程池源码**](https://javadoop.com/post/java-thread-pool))

#### synchized和lock什么区别？ 底层细节

- ([**Synchronized**](https://juejin.im/post/5b4eec7df265da0fa00a118f))

- ([内存可见性](https://javadoop.com/post/java-memory-model))

#### threadLocal是什么？底层如何实现？写一个例子

- ([**threadLocal基本原理**](https://blog.csdn.net/u010887744/article/details/54730556))

#### volitile的工作原理

- ([volitile基本原理](https://www.cnblogs.com/dolphin0520/p/3920373.html))

#### cas知道吗？如何实现？

- ([**cas**](http://zl198751.iteye.com/blog/1848575))

#### 四种写法，写一个单例模式

- ([彻底理解单例模式](https://blog.csdn.net/justloveyou_/article/details/64127789))

- ([设计模式](https://www.cnblogs.com/aspirant/category/1024672.html))

#### jvm内存模型？用过哪些垃圾回收器？说说

- ([**java内存模型**](https://juejin.im/post/5b7d69e4e51d4538ca5730cb))

- ([**Java8内存模型—永久代(PermGen)和元空间(Metaspace)**](http://www.cnblogs.com/paddix/p/5309550.html))

- ([**常量区**](https://blog.csdn.net/qq_26222859/article/details/73135660))

#### 线上频繁发生fullGC 如何处理？ CPU使用频率过高怎么办？

- ([**Java GC是在什么时候，对什么东西，做了什么事情**](https://my.oschina.net/hosee/blog/674314))

- ([**什么时候会发生FullGC**](https://my.oschina.net/hosee/blog/674261))

- ([**使用CMS垃圾收集器产生的问题和解决方案**](https://my.oschina.net/hosee/blog/674181))

- ([垃圾回收机制](https://javadoop.com/post/jvm-memory-management))

- ([G1](https://javadoop.com/post/g1))

#### Integer x= 5, int y=5 比较x==y有哪些步骤

- ([**原生类型和包装器类型**](https://juejin.im/post/5ad158d2f265da2381560d83))

#### 类加载机制？ 类加载器？分别加载哪些文件？手写一个

- ([**加载器原理**](https://blog.csdn.net/justloveyou_/article/details/72217806))

- ([图解tomcat类加载](https://www.cnblogs.com/aspirant/p/8991830.html))

- ([**类加载时机和过程**](https://blog.csdn.net/justloveyou_/article/details/72466105))

- ([**对象初始化**](https://blog.csdn.net/justloveyou_/article/details/72466416))

**jvm优化？使用什么方法？达到什么效果**

- ([jvm](https://www.cnblogs.com/aspirant/category/1195271.html))

#### spring 哪些机制？aop如何实现？IOC如何实现

- ([**自己实现AOP和IOC**](https://www.cnblogs.com/aspirant/p/9187973.html))

- ([**ioc原理**](https://blog.csdn.net/it_man/article/details/4402245))

- ([IOC源码分析](https://javadoop.com/post/spring-ioc))

- ([AOP源码分析](https://javadoop.com/post/spring-aop-source))


#### https

- ([https原理](https://juejin.im/post/5a2ff29c6fb9a045132aac5a))

#### git原理

- ([git原理](https://juejin.im/post/5a65ac67f265da3e330473f7))

#### 子网掩码

- ([子网掩码的原理和应用](https://blog.csdn.net/faker_wang/article/details/80747407))

#### 动态代理

- ([**基于CGLIB**](https://juejin.im/post/5b3e05caf265da0f652364ce))

- ([**基于JDK**](https://juejin.im/post/5b39dee0e51d4558cc35e3a5))

- ([**代码实现**](https://my.oschina.net/hosee/blog/656945))

#### NIO

- ([javaNIO](https://juejin.im/post/5b21d775e51d4506b53ec412))

- ([NIO细节](https://javadoop.com/post/java-nio))

- ([Java 非阻塞 IO 和异步 IO](https://javadoop.com/post/nio-and-aio))

- ([tomcat中的应用](https://javadoop.com/post/tomcat-nio))

#### dump

- ([dump使用](https://www.cnblogs.com/aspirant/p/8881047.html))


#### 基于socket的网络编程

- ([socket](https://blog.csdn.net/zhangchenghaopeng/article/details/50571079))

#### javaCore

- ([**序列化**](https://juejin.im/post/5b4c69dcf265da0fa959aa06))

- ([位运算](https://blog.csdn.net/xiaopihaierletian/article/details/78162863))


### 链接收集

- ([大厂面试集锦](https://www.cnblogs.com/aspirant/p/8575628.html))
- ([备战阿里](http://www.cnblogs.com/zhengbin/category/787240.html))
- ([数据库面试大全](https://www.cnblogs.com/aspirant/category/773065.html))
- ([数据结构](https://www.cnblogs.com/aspirant/category/560576.html))
- ([计算机网络面试大全](https://www.cnblogs.com/aspirant/category/581827.html))
- ([spring](https://www.cnblogs.com/aspirant/category/1012476.html))
- ([java core](https://www.cnblogs.com/skywang12345/archive/2013/06/14/index.html))
