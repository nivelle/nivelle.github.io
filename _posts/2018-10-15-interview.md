---
layout: post
title:  "java必知必会"
date:   2018-10-15 00:06:05
categories: 技术
tags: interview
excerpt: interview
---

* content
{:toc}

## java interview

### 数据库相关
#### 使用mysql索引都有哪些原则？索引有什么数据结构？ B+tree 和 B tree 什么区别
- ([**mysql索引章节**](http://nivelle.me/2017/05/04/索引/))
- ([mysql索引实现](https://www.cnblogs.com/zlcxbb/p/5757245.html))
- ([索引使用](https://juejin.im/post/5b14e0fd6fb9a01e8c5fc663))
#### mysql 有哪些存储引擎？有什么区别？
- ([**mysql索引类型比较**](https://www.cnblogs.com/changna1314/p/6878900.html)) 
- ([mysql索引类型以及特点](https://my.oschina.net/junn/blog/183341))
- ([**mysqlAUTO_INCREMENT**](https://blog.csdn.net/iamczb/article/details/43112689))
- ([mysql优化原理](https://www.jianshu.com/p/d7665192aaaf))
#### 设计高并发系统数据库层面应该怎么设计？ 数据库锁有哪些类型？如何实现
- ([高并发系统设计-数据层](https://blog.csdn.net/chenpeng19910926/article/details/51789934))
- ([数据库高并发方案](https://blog.csdn.net/wangkehuai/article/details/46727203))
- ([**mysql高并发方案**](https://blog.csdn.net/kidoo1012/article/details/54691561))
- ([架构设计 数据库方案](https://www.w3cschool.cn/architectroad/architectroad-arv823bf.html))
#### 数据库事物有哪些？
- ([**数据库事务基础**](http://nivelle.me/2017/06/05/%E4%BA%8B%E7%89%A9%E5%AD%A6%E4%B9%A0(%E4%B8%80)%E4%B9%8B%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5/))
- ([**数据库事务使用**](http://nivelle.me/2017/06/10/%E4%BA%8B%E5%8A%A1%E4%BD%BF%E7%94%A8/))
- ([**mysql日志类型**](https://www.cnblogs.com/wy123/p/8365234.html))
#### 如何设计可以动态扩容的分库分表方案？ 以及底层原理？常见的分库分表中间件？优缺点？ 如何让未分库分表的数据动态切换到分库分表的系统上？分库分表解决主键问题?
- ([**分库分表方案以及问题**](https://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=2650994413&idx=1&sn=24a01089ee47793b5d82381b04a34499&chksm=bdbf0ebe8ac887a8a75a0cd9226bb7e0427f1a77c43323d14dc6932c7dc77555531bce5dce0f&scene=0))
- ([**分库分表具体方案**](https://mp.weixin.qq.com/s?__biz=MzUxNTU4NjAwMw==&mid=2247483669&idx=1&sn=b1e63b6734ba4b43095b9c70fa97276f&chksm=f9b523a9cec2aabf0a2acc632e1c1b0510c95a3afa21ed1c5a1d21f6378ccfbe6d48624cffa7&scene=21#wechat_redirect))
#### 分布式事物？如何实现？TCC？ 网络出现问题，如何容错？
- ([**2pc和3pc**](https://www.jianshu.com/p/6fb2c5b1b664))
- ([**分布式系统事务一致性解决方案**](http://www.infoq.com/cn/articles/solution-of-distributed-system-transaction-consistency#))
- ([TCC](https://juejin.im/post/5a74f3bc6fb9a0633f0df127))
- ([**分布式事务实现**](https://blog.csdn.net/tzs_1041218129/article/details/80086991))
#### 分布式寻址方式方式有哪些算法？ 一致性hash算法
- （[**一致性hash算法**](http://www.zsythink.net/archives/1182)）
- （[递归算法](https://blog.csdn.net/justloveyou_/article/details/71787149))
---
## 缓存相关
#### redis和 memcheched什么区别？为什么单线程的redis比多线程的memched效率高？
- ([**redis的特点和原理**](https://juejin.im/post/5ad6e4066fb9a028d82c4b66))
- ([**redis和memcheched的比较**](https://www.imooc.com/article/23549))
#### redis主要数据类型？分别那种场景下使用？
- ([**redis实战**](http://nivelle.me/2019/09/29/redis%E5%AE%9E%E6%88%98/))
#### redis的主从复制怎么实现的？redis集群模式是如何实现的？ redis的key是如何寻址的？
- ([**redis主从复制**](https://my.oschina.net/zhaolin/blog/752520))
- ([**redis集群和哨兵模式配置**](https://www.cnblogs.com/jaycekon/p/6237562.html))
#### redis基本知识
- ([**redis基本知识**](https://github.com/CyC2018/CS-Notes/blob/master/notes/Redis.md#%E5%8D%81%E4%B8%89%E6%95%B0%E6%8D%AE%E6%B7%98%E6%B1%B0%E7%AD%96%E7%95%A5))
- ([**redis单线程**](https://mp.weixin.qq.com/s?__biz=MzUxNTU4NjAwMw==&mid=2247483725&idx=1&sn=e6c8a304d1618b63bf04a264ba604585&chksm=f9b523f1cec2aae7c771af0e157f819d88abdf8bdfa483838279b51fbb0f23ab2fdd32c5ae50&mpshare=1&scene=1&srcid=1009czaemdDdwr7Y04gSPbaj#rd))
- ([**缓存概念**](https://blog.csdn.net/xlgen157387/article/details/79530877))
#### 缓存如何使用？缓存使用不当带来什么问题
- ([缓存界的三大问题](https://juejin.im/post/5aa8d3d9f265da2392360a37))
---
## 分布式架构相关
#### zk原理? zk的应用？ paxos算法？
- ([**zk原理**](http://nivelle.me/2016/04/12/Zookeeper%E5%AD%A6%E4%B9%A0-%E4%BA%8C-%E4%B9%8Bzk%E5%9F%BA%E7%A1%80/))
- ([**zk应用**](http://nivelle.me/2017/04/12/Zookeeper%E5%AD%A6%E4%B9%A0-%E4%B8%89-%E4%B9%8Bzk%E7%9A%84%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF/))
- ([**paxos算法**](https://www.cnblogs.com/linbingdong/p/6253479.html))
- ([zk概述](https://www.cnblogs.com/felixzh/p/5869212.html))
- ([**zk启动过程分析**](http://shift-alt-ctrl.iteye.com/blog/1846507))
- ([zk选举过程分析](http://shift-alt-ctrl.iteye.com/blog/1846562))
- ([zkWatcher机制分析](http://shift-alt-ctrl.iteye.com/blog/1847320))
#### dubbo的实现过程？注册中心挂了可以继续通信么？dubbo常见配置有哪些
- ([**dubbo常见配置**](http://www.cnblogs.com/yxh1008/p/9251693.html))
#### dubbo支持哪些序列化协议？hession?hession数据结构？ pb知道么？为啥PB的效率是最高的？
- ([**dubbo学习**](https://www.cnblogs.com/aspirant/p/9002631.html))
- ([**xstream、protobuf、protostuff**](https://www.cnblogs.com/xiaoMzjm/p/4555209.html))
#### dubbo负载均衡策略和高可用策略有哪些？动态代理策略呢?为什么要进行系统拆分啊？ 拆分不用dubbo可以么？ dubbo和thirft什么区别？
- ([**rpc框架**](http://www.cnblogs.com/aspnet2008/p/?page=1))
- ([**dubbo的负载均衡**](https://www.cnblogs.com/aspirant/p/8994227.html))
- ([**dubbo知识点**](https://mp.weixin.qq.com/s/PdWRHgm83XwPYP08KnkIsw))
- ([**dubbo实现**](https://blog.csdn.net/Revivedsun/article/details/74514078)）
#### 不用应用环境下的会话保持
- ([会话保持](http://blog.51cto.com/virtualadc/592454))
#### 自己实现RPC
- ([**Dubbo原理简介**](http://www.cnblogs.com/LBSer/p/4853234.html))
- ([**轻量级分布式 RPC 框架**](https://my.oschina.net/huangyong/blog/361751))
- ([**简单rpc**](http://www.cnblogs.com/codingexperience/p/5930752.html))
- ([dubbo源码](https://my.oschina.net/ywbrj042?tab=popular))
#### 如何设计一个高并发高可用系统？
- ([**高并发**](https://www.w3cschool.cn/architectroad/architectroad-high-concurrent.html))
- ([**高可用**](https://www.w3cschool.cn/architectroad/architectroad-high-availability.html))
- ([**大型网站架构演化历程**](http://www.hollischuang.com/archives/728))
#### 如何限流？工程中怎么做的？说下具体实现？
- ([**限流**](https://www.w3cschool.cn/architectroad/architectroad-optimization-of-seckilling-system.html))
- ([**常见限流方案**](http://manzhizhen.iteye.com/blog/2311691))
#### 负载均衡
- ([**六大负载均衡原理**](https://www.cnblogs.com/aspirant/p/9087716.html))
- ([lvs](https://www.cnblogs.com/aspirant/p/9084740.html))
#### 如何降级？如何进行系统拆分，如何进行数据库拆分
- ([服务降级](http://jinnianshilongnian.iteye.com/blog/2306477))
----
## 消息相关
#### netty 可以干什么？ NIO，BIO ，AIO 都是什么？ 有什么区别
- ([**NIO,BIO,AIO**](https://www.cnblogs.com/aspirant/p/6877350.html))
#### 为什么使用消息队列？消息队列的优点和缺点？
- ([消息队列的意义](https://www.w3cschool.cn/architectroad/architectroad-message-queue.html))
#### 如何保证消息队列的高可用？如何保证消息不被重复消费？
- ([**消息队列的幂等**](https://www.w3cschool.cn/architectroad/architectroad-message-idempotence.html))
- ([**消息队列的高可用**](https://www.w3cschool.cn/architectroad/architectroad-message-delivery.html))
#### kafka，activemq,rabbitmq,rocketmq 都有什么优点和缺点？如何自己设计一个消息队列，该如何进行架构设计
- ([消息队列大全](https://www.cnblogs.com/aspirant/category/1195858.html))
- ([消息队列细节学习](https://blog.csdn.net/u013256816))
- ([kafka数据一致性和zk的比较](https://www.cnblogs.com/aspirant/p/9179045.html))
#### rabbitMQ基础
- ([rabbitMQ](https://www.kancloud.cn/longxuan/rabbitmq-arron/117513))
---
## 算法数据结构
#### 海量数据如何去重？
- ([海量数据去重](https://blog.csdn.net/paul_wei2008/article/details/21170999))
- ([大数据排序](https://blog.csdn.net/michellechouu/article/details/27230451))
- ([海量数据找topK](https://www.cnblogs.com/DarrenChan/p/8796749.html))
#### 排序算法
- ([堆排序](https://blog.csdn.net/Sun_Ru/article/details/52004044))
- ([**冒泡排序**](https://blog.csdn.net/IT_ZJYANG/article/details/51010651))
- ([**快速排序**](https://blog.csdn.net/IT_ZJYANG/article/details/53406764))
#### 常见数据结构
- ([常见数据结构](https://blog.csdn.net/fighterandknight/article/category/6193841))
- ([常见数据结构2](https://blog.csdn.net/zxt0601/article/category/6697194))
- ([**arrayList线程不安全分析**](https://www.jianshu.com/p/41be1efe5d65))
- ([**moudCount的作用**](https://blog.csdn.net/qq_24235325/article/details/52450331))
- ([**CopyOnWriteArrayList**](https://blog.csdn.net/linsongbin1/article/details/54581787))
- ([**hashtable和hashMap**](https://blog.csdn.net/tgxblue/article/details/8479147))
- ([**hashMap非线程安全分析**](http://www.importnew.com/22011.html))
- ([**hashMap key==null 分析**](https://blog.csdn.net/glory1234work2115/article/details/50825503))
- ([**String**](https://blog.csdn.net/justloveyou_/article/details/52556427))
- ([**hashMap分析**](http://note.youdao.com/noteshare?id=e5a60b1100b28bb62c7b9a101efe477c))
- ([**hashMap分析2**](https://blog.csdn.net/tuke_tuke/article/details/51588156))
- ([**ConcurrentHashMap分析**](http://www.importnew.com/28263.html))
- ([stack实现](https://blog.csdn.net/javazejian/article/details/53362993))
- ([树结构](https://blog.csdn.net/DouBoomFly/article/details/70171410))
- ([**二叉树**](https://blog.csdn.net/xiaoquantouer/article/details/65631708?utm_source=blogxgwz0))
- ([**B树**](https://blog.csdn.net/guoziqing506/article/details/64122287?utm_source=blogxgwz8))
---
## java基础相关
#### hashcode相等两个类一定相等吗？ equals 呢 相反呢？
- ([**hashCode()和equals()**](https://www.cnblogs.com/skywang12345/p/3324958.html))
#### 线程池用过么？都有哪些参数？ 底层如何实现？
- ([多线程面试](https://www.cnblogs.com/aspirant/category/1017858.html))
- ([**任务的抽象**](https://blog.csdn.net/justloveyou_/article/details/79846241))
- ([**线程池**](https://blog.csdn.net/fngy123/article/details/48738409))
- ([AQS源码](https://javadoop.com/post/AbstractQueuedSynchronizer))
- ([**线程池源码**](https://javadoop.com/post/java-thread-pool))
#### synchized和lock什么区别？ 底层细节
- ([**Synchronized**](https://juejin.im/post/5b4eec7df265da0fa00a118f))
- ([**内存可见性**](https://javadoop.com/post/java-memory-model))
- ([**java锁保证内存可见性**](http://ifeve.com/java%E9%94%81%E6%98%AF%E5%A6%82%E4%BD%95%E4%BF%9D%E8%AF%81%E6%95%B0%E6%8D%AE%E5%8F%AF%E8%A7%81%E6%80%A7%E7%9A%84/))
#### threadLocal是什么？底层如何实现？写一个例子
- ([**threadLocal基本原理**](http://www.iteye.com/topic/103804))
#### volitile的工作原理
- ([**volitile基本原理**](http://www.hollischuang.com/archives/2673))
- ([**深入理解volatile**](http://www.hollischuang.com/archives/2648))
#### Thread and lock
- ([**thread and lock**](https://javadoop.com/post/Threads-And-Locks-md#17.5.%20final%20属性的语义（final%20Field%20Semantics）))
#### cas知道吗？如何实现？
- ([**cas**](http://zl198751.iteye.com/blog/1848575))
#### 四种写法，写一个单例模式
- ([**彻底理解单例模式**](https://blog.csdn.net/justloveyou_/article/details/64127789))
- ([设计模式](https://www.cnblogs.com/aspirant/category/1024672.html))
#### Integer x= 5, int y=5 比较x==y有哪些步骤
- ([**原生类型和包装器类型**](https://juejin.im/post/5ad158d2f265da2381560d83))
#### javaCore
- ([**序列化**](https://juejin.im/post/5b4c69dcf265da0fa959aa06))
- ([位运算](https://blog.csdn.net/xiaopihaierletian/article/details/78162863))
#### 动态代理
- ([**基于CGLIB**](https://juejin.im/post/5b3e05caf265da0f652364ce))
- ([**基于JDK**](https://juejin.im/post/5b39dee0e51d4558cc35e3a5))
- ([**代码实现**](https://my.oschina.net/hosee/blog/656945))
#### NIO
- ([javaNIO](https://juejin.im/post/5b21d775e51d4506b53ec412))
- ([NIO细节](https://javadoop.com/post/java-nio))
- ([**Java 非阻塞 IO 和异步 IO**](https://javadoop.com/post/nio-and-aio))
- ([tomcat中的应用](https://javadoop.com/post/tomcat-nio))
#### 基于socket的网络编程
- ([socket](https://blog.csdn.net/zhangchenghaopeng/article/details/50571079))
---
## jvm相关
#### jvm内存模型？用过哪些垃圾回收器？说说
- ([**java内存模型**](https://juejin.im/post/5b7d69e4e51d4538ca5730cb))
- ([**Java8内存模型—永久代(PermGen)和元空间(Metaspace)**](http://www.cnblogs.com/paddix/p/5309550.html))
- ([**常量区**](https://blog.csdn.net/qq_26222859/article/details/73135660))
- ([jvm常见配置](https://www.cnblogs.com/parryyang/p/5750146.html))
#### 线上频繁发生fullGC 如何处理？ CPU使用频率过高怎么办？
- ([**Java GC是在什么时候，对什么东西，做了什么事情**](https://my.oschina.net/hosee/blog/674314))
- ([**什么时候会发生FullGC**](https://my.oschina.net/hosee/blog/674261))
- ([**使用CMS垃圾收集器产生的问题和解决方案**](https://my.oschina.net/hosee/blog/674181))
- ([**垃圾回收机制**](https://javadoop.com/post/jvm-memory-management))
- ([**G1收集器**](https://javadoop.com/post/g1))
- ([经典回答](https://www.zhihu.com/question/53613423))
#### 类加载机制？ 类加载器？分别加载哪些文件？手写一个
- ([**加载器原理**](https://blog.csdn.net/justloveyou_/article/details/72217806))
- ([图解tomcat类加载](https://www.cnblogs.com/aspirant/p/8991830.html))
- ([**类加载时机和过程**](https://blog.csdn.net/justloveyou_/article/details/72466105))
- ([**对象初始化**](https://blog.csdn.net/justloveyou_/article/details/72466416))
#### jvm优化？使用什么方法？达到什么效果**
- ([jvm](https://www.cnblogs.com/aspirant/category/1195271.html))
- ([java调优](https://my.oschina.net/feichexia/blog/196575))
- ([java常见调优](http://unixboy.iteye.com/blog/174173/))
#### 排查过程
- ([死循环](https://mp.weixin.qq.com/s?__biz=MzI1NDQ3MjQxNA==&mid=2247487380&idx=1&sn=e8c350a946696940dc4c13b0b7fcfcd3&chksm=e9c5f625deb27f331eefc431a10d7c90bef50a2afd2c4f9299d2eb514982c6623b0569cf0e19&mpshare=1&scene=1&srcid=#rd))
---
## 框架相关
#### spring 哪些机制？aop如何实现？IOC如何实现
- ([**自己实现AOP和IOC**](https://www.cnblogs.com/aspirant/p/9187973.html))
- ([**IOC原理**](https://blog.csdn.net/it_man/article/details/4402245))
- ([**IOC源码分析**](https://javadoop.com/post/spring-ioc))
- ([AOP源码分析](https://javadoop.com/post/spring-aop-source))
- ([**spring头文件原理**](https://www.cnblogs.com/mesopotamia/p/4948861.html))
- ([**spring初始化bean控制**](https://zhuanlan.zhihu.com/p/30112785))
#### springboot
- ([**springboot原理**](https://blog.csdn.net/hengyunabc/article/details/50120001))
---
## 网络相关
#### https
- ([**https原理**](https://juejin.im/post/5a2ff29c6fb9a045132aac5a))
#### http的工作流程？越细节越好。  http1.0 1.1 1.2有哪些区别
- ([**一次url请求**](https://github.com/skyline75489/what-happens-when-zh_CN))
#### tcp 三次握手，四层分手的工作流程〉为什么不是其他次数？
- ([**计算机网络面试常见**](https://www.jianshu.com/p/d0dd47afabad))
####  get/post区别
- ([**get/post**](https://juejin.im/entry/599eb96ef265da24722fc15b))
####  cookie 和 session
- ([**安全性**](https://segmentfault.com/q/1010000007347730))
#### OAuth2.0
- ([OAuth2.0原理](https://my.oschina.net/wangzhenchao/blog/851773))
#### 子网掩码
- ([**子网掩码的原理和应用**](https://blog.csdn.net/faker_wang/article/details/80747407))
#### dump
- ([dump使用](https://www.cnblogs.com/aspirant/p/8881047.html))
---
## 其他
#### git原理
- ([git原理](https://juejin.im/post/5a65ac67f265da3e330473f7))
#### 常见加解密算法
- ([**加解密**](https://blog.csdn.net/u013565368/article/details/53081195))
#### 注解原理
- ([**注解原理**](https://blog.csdn.net/zhang0558/article/details/52643016))
---
## 附录
### 阿里面试题以及回答要点

- java中常用的数据结构，比如hashMap,你给我讲一下hashMap的原理，以及他的结构，如果发生了HashCode冲突怎么办？你再和我讲一下，线程安全的HashMap怎么创建，他们各自的原理是什么，你详细的说一下。

```
数组+链表+红黑树,16,与运算,减少冲突,头插法,resize()线程不安全

concurrentHashMap ,cas操作保证线程安全. 扩容:8,64

arrayLiat线程不安全,10,第一次扩容10;add(),ensureExplicitCapacity()

```

- 分布式协调一致性，你给我讲一下zk集群选举leader的算法，它是怎么推算出leader和follower的？


```
节点状态:looking，leading,following,observing .   

epoch>zxid>zsid


```



- 你知道我们创建线程的方法有很多种，比如线程池，他的参数有哪些，具体说说这些参数的作用？你和给我讲一下他的原理吗？它是怎么实现的？底层是什么样子的，
你能自己实现一个线程池吗？怎么实现？

```
corepoorSize,maximumPoolSize,keepAliveTime,unit,

BlockingQueue<Runnable>,ThreadFactory,

RejectedExecutionHandler

小于corePoolSize 创建新线程,如果线程数达到corePoolSize，我们的每个任务会提交到等待队列中，等待线程池中的线程来取任务并执行,如果队列满,创建新线程maximumPoolSize,有keepAliveTime,然后有拒绝策略.



```

- 你知道java中有好多数据结构，比如二叉树，你和能我讲一下红黑树和平衡二叉树的原理吗？具体怎么实现的？

```


```

- 数据量大了以后，我们通常会使用MySQL主从配置，读写分离，你能和我讲一下mysql主从配置，读写分离的原理吗？它的底层是怎么实现的？

```
binlog 数据同步主从复制; 读写分离:代码层,代理控制.

```

- redis集群，你知道我们通常会使用redis集群，当时他的伸缩性不是很好，当你增加或者删除一个节点的时候，他需要重新计算数据的存放位置，你能设计一个算法，让他尽可能的少挪动原来节点中的数据吗？

```
哨兵模式

```

- 你能现场给我设计一个用于高并发的秒杀的系统吗？给我说说你的设计？

```

缓存，预热

```

- 实现线程安全的方式？你把你知道的方法都说出来，比如synchronized，lock等等，你还知道哪些，java中有好多线程安全的封装类，你用过哪些，和我说说，为什么使用它？你知道volatile这个关键词吗？你和我说说他的原理？

```
本质原因是多缓存。 主内存和本地内存 。 volatile:内存可见行，禁止指令重排序。final内存可见性。 内存屏障。 synchronized:monitorEnter,mointerExit. locK:cas. 

final 属性同时也允许程序员不需要使用同步就可以实现线程安全的不可变对象。

```

- 你和我说说jvm运行时的数据区吧，你说说你知道哪些，把你知道的都说出来？哪些是线程共享的，哪些是私有的？

```
计数器，本地方法栈，java虚拟机栈,方法区，堆。

```

- 你和我说说Garbage Collection吧，把你知道的全部说出来。包括回收的方法，怎么使用？什么时候使用，原理等。

```
1. 大部分对象都是短命的，它们在年轻的时候就会死去 2.极少老年对象对年轻对象的引用。 新生代考虑的是算法的速度，因为新生代垃圾回收是频繁的；老年代，需要考虑的是空间，因为老年代占用了大部分的堆内存。

串行收集器：在年轻代和老年代都采用单线程，年轻代中使用 stop-the-world、复制 算法；老年代使用 stop-the-world、标记 -> 清理 -> 压缩 算法。

并行收集器：在年轻代中使用 并行、stop-the-world、复制 算法；老年代使用串行收集器的 串行、stop-the-world、标记 -> 清理 -> 压缩 算法。

并行压缩收集器：在年轻代中使用并行收集器的 并行、stop-the-world、复制 算法；老年代使用 并行、stop-the-world、标记 -> 清理 -> 压缩 算法。和并行收集器的区别是老年代使用了并行。

CMS 收集器：在年轻使用并行收集器的 并行、stop-the-world、复制 算法；老年代使用 并发、标记 -> 清理 算法，不压缩。本文介绍的唯一一个并发收集器，也是唯一一个不对老年代进行压缩的收集器。

避免fullGC:增加堆大小，或调整老年代和年轻代的比例,增加并发周期的线程数量，其实就是为了加快并发周期快点结束,让并发周期尽早开始，这个是通过设置堆使用占比来调整的（默认 45%）,在混合垃圾回收周期中回收更多的老年代区块

```

- 你和我说说数据库瓶颈把，说说哪些怎么分区？你们之前是怎么分区的？为什么要这么做？

```
数据库IO

```

- 分布式的情况下，有时候我们需要实现锁，你知道怎么实现吗？把你知道的说出来？不同的方法，分别适用于那些场景

```
分布式锁必须有的特点：可重入（避免死锁），同一个方法在同一时间只能被同一台机器的一个线程执行，可阻塞，高可用的获取锁和释放锁，有失效策略。

数据库表（单点问题，非重入，for update 实现可阻塞/具有失效策略），redis , zk(零时顺序节点，集群部署，watcher时间)

```

- 你和我说说数据库的乐观锁和悲观锁把，他们怎么实现的，具体应用在哪些场景，怎么使用。

```
乐观锁：基于竞争不是很激烈，cas,version  悲观锁：互斥，上下文切换调度延时

```

- 你和我说说spring把，你把ioc和依赖注入和我讲一下，它是怎么实现的？再说说AOP，什么时候会用到aop，业务场景有哪些。他的原理是什么？有哪几种advice。

```
IOC : Inversion of Control  AOP:Aspect-Oriented Programming

```

- 你给我讲讲缓存穿透把，你知道什么是缓存穿透吗？讲一下，然后和我说一下怎么解决？

```
业务系统本身就不存在的数据。 1.缓存空数据（数据库查询结果为空的key也存储在缓存中）2.boomFilter(存所有可能的key不存在直接返回)

```

- 业务高并发时，你有什么好的方法解决？说出你知道的一些方法。

```
响应时间，吞吐量，QPS，并发用户数。 垂直扩展，水平扩展。反向代理层(dns,nginx)，站点层（web）,服务层（服务连接池）,数据层（范围，哈希）

高可用：冗余+故障自动转移。 反向代理层（keepalived存活探测，相同virtual IP提供服务），站点层（nginx配置多个后端，自动探测活性），服务层（service-connection-pool）,缓存层（1. service对cache进行双读双写 2.哨兵模式），数据层（主从同步，读写分离，双主同步）

```

- 给你两个链表，一个长度为m，一个长度为n，请你设计一个算法，使得时间复杂度最低，算出他们的交集，并且告诉我你的时间复杂度。

- 给我讲一下快速排序，他的原理。怎么实现的。

```
基准 ， 左右同时递归，O(NlogN)/O(N*N)

```

---
### 链接收集
- ([大厂面试集锦](https://www.cnblogs.com/aspirant/p/8575628.html))
- ([备战阿里](http://www.cnblogs.com/zhengbin/category/787240.html))
- ([java core](https://www.cnblogs.com/skywang12345/archive/2013/06/14/index.html))






