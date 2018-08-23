---
layout: post
title:  "开发人员必知必会"
date:   2018-08-09 00:06:05
categories: 技术
tags: interview
excerpt: interview
---

* content
{:toc}



#### **使用mysql索引都有哪些原则？索引有什么数据结构？ B+tree 和 B tree 什么区别**

- ([mysql索引章节](http://nivelle.me/2017/05/04/索引/))

- ([mysql树结构](http://blog.jobbole.com/111680/))

- ([mysql索引实现](https://www.cnblogs.com/zlcxbb/p/5757245.html))
- ([索引使用](https://juejin.im/post/5b14e0fd6fb9a01e8c5fc663))
  
#### **mysql 有哪些存储引擎？有什么区别？**

- ([mysql索引类型比较](https://www.cnblogs.com/changna1314/p/6878900.html)) 

- ([mysql索引类型以及特点](https://my.oschina.net/junn/blog/183341))

- ([mysqlAUTO_INCREMENT](https://blog.csdn.net/zm2714/article/details/8482211))

#### **设计高并发系统数据库层面应该怎么设计？ 数据库锁有哪些类型？如何实现**

- ([高并发系统设计-数据层](https://blog.csdn.net/chenpeng19910926/article/details/51789934))

- ([mysqlAUTO_INCREMENT](https://blog.csdn.net/wangkehuai/article/details/46727203))
- ([mysql 高并发方案](https://blog.csdn.net/kidoo1012/article/details/54691561))

- ([架构设计 数据库方案](https://www.w3cschool.cn/architectroad/architectroad-arv823bf.html))


#### **数据库事物有哪些**？

- ([数据库事务基础](http://nivelle.me/2017/06/05/%E4%BA%8B%E7%89%A9%E5%AD%A6%E4%B9%A0(%E4%B8%80)%E4%B9%8B%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5/))

- ([数据库事务使用](http://nivelle.me/2017/06/10/%E4%BA%8B%E5%8A%A1%E4%BD%BF%E7%94%A8/))

#### **如何设计可以动态扩容的分库分表方案？ 以及底层原理？常见的分库分表中间件？优缺点？ 如何让未分库分表的数据动态切换到分库分表的系统上？分库分表解决主键问题**?

- ([分库分表方案](https://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=2650994413&idx=1&sn=24a01089ee47793b5d82381b04a34499&chksm=bdbf0ebe8ac887a8a75a0cd9226bb7e0427f1a77c43323d14dc6932c7dc77555531bce5dce0f&scene=0))


#### **分布式事物？如何实现？TCC？ 网络出现问题，如何容错**？

- ([分布式事务](http://www.infoq.com/cn/articles/solution-of-distributed-system-transaction-consistency#))
- ([分布式事务阶段提交](http://int64.me/2016/%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%8B%E5%8A%A12PC%20&&%203PC.html))
- ([TCC](https://juejin.im/post/5a74f3bc6fb9a0633f0df127))

#### **分布式寻址方式方式有哪些算法**？ **一致性hash算法**

- ([一致性hash算法](http://www.zsythink.net/archives/1182))

#### **redis和 memcheched什么区别？为什么单线程的redis比多线程的memched效率高？**

- ([redis的特点和原理](https://juejin.im/post/5ad6e4066fb9a028d82c4b66))

#### **redis主要数据类型？分别那种场景下使用**？

 - ([动态字符串](http://nivelle.me/2017/07/24/redis%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0(%E4%B8%80)/))

- ([链表](http://nivelle.me/2017/07/24/redis%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0(%E4%BA%8C)%E4%B9%8B%E9%93%BE%E8%A1%A8/))

- ([跳跃表](http://nivelle.me/2017/09/07/redis%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0(%E4%B8%89)%E4%B9%8B%E8%B7%B3%E8%B7%83%E8%A1%A8/))
- ([字典](http://nivelle.me/2017/09/04/redis%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0(%E5%9B%9B)%E4%B9%8B%E5%AD%97%E5%85%B8/)) 


**redis的主从复制怎么实现的？redis集群模式是如何实现的？ redis的key是如何寻址的**？

**使用redis如何设计分布式锁？ 使用zk可以吗？如何实现？两种效率更高**？

#### **redis持久化？各有什么优缺点？具体底层实现**？
- ([RDB](http://nivelle.me/2017/09/18/redis%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0(%E5%8D%81%E5%9B%9B)%E4%B9%8BRDB/)) 
- ([AOF](http://nivelle.me/2017/09/22/redis%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0(%E5%8D%81%E4%BA%94)%E4%B9%8BAOF/)) 

**redis过期策略？lru ? 写下java版本的代码**？

**dubbo的实现过程？注册中心挂了可以继续通信么？dubbo常见配置有哪些**

#### **zk原理? zk的应用？ paxos算法**？

- ([zk原理](http://nivelle.me/2016/04/12/Zookeeper%E5%AD%A6%E4%B9%A0-%E4%BA%8C-%E4%B9%8Bzk%E5%9F%BA%E7%A1%80/))

- ([zk应用](http://nivelle.me/2017/04/12/Zookeeper%E5%AD%A6%E4%B9%A0-%E4%B8%89-%E4%B9%8Bzk%E7%9A%84%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF/))

- ([paxos算法](https://www.cnblogs.com/linbingdong/p/6253479.html))

#### **dubbo支持哪些序列化协议？hession?hession数据结构？ pb知道么？为啥PB的效率是最高的**？
- ([dubbo原理](https://juejin.im/post/5b1203f2e51d450689495cea))

**netty 可以干什么？ NIO，BIO ，AIO 都是什么？ 有什么区别**

**dubbo负载均衡策略和高可用策略有哪些？动态代理策略呢**？

**为什么要进行系统拆分啊？ 拆分不用dubbo可以么？ dubbo和thirft什么区别**？

#### **为什么使用消息队列？消息队列的优点和缺点**？

- （[消息队列的意义](https://www.w3cschool.cn/architectroad/architectroad-message-queue.html)）

#### **如何保证消息队列的高可用？如何保证消息不被重复消费**？

- ([消息队列的幂等](https://www.w3cschool.cn/architectroad/architectroad-message-idempotence.html))
- ([消息队列的高可用](https://www.w3cschool.cn/architectroad/architectroad-message-delivery.html))

**kafka，activemq,rabbitmq,rocketmq 都有什么优点和缺点**？

**如何自己设计一个消息队列，该如何进行架构设计**

**如何设计一个高并发高可用系统？**

#### **如何限流？工程中怎么做的？说下具体实现**？

- ([限流](https://www.w3cschool.cn/architectroad/architectroad-optimization-of-seckilling-system.html))

**缓存如何使用？缓存使用不当带来什么问题**？

#### **如何降级？如何进行系统拆分，如何进行数据库拆分**？

- ([服务降级](http://jinnianshilongnian.iteye.com/blog/2306477))

#### **http的工作流程？越细节越好。  http1.0 1.1 1.2有哪些区别**
- ([http工作流程](https://zrj.me/archives/tag/%E4%BB%8E%E7%82%B9%E5%87%BB%E5%88%B0%E5%91%88%E7%8E%B0))
- ([http工作细节](https://juejin.im/post/5b10be81518825139e0d8160))
#### **tcp 三次握手，四层分手的工作流程〉为什么不是其他次数**？

- ([计算机网络面试常见](https://www.jianshu.com/p/d0dd47afabad))

**45亿阿拉伯数据，如何进行去重复？如何找出最大的那个**？

**hashcode相等两个类一定相等吗？ equals 呢？ 相反呢**？

#### **介绍下集合框架**
- ([容器](https://blog.csdn.net/u013256816/article/details/50925091))

#### **hashmap和hashtable底层实现？ hashtable 和 concurrenthashtable呢**？

- ([hashMap](http://nivelle.me/2017/11/28/%E5%B8%B8%E8%A7%81%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84(1)-hashMap/))

- ([ConcurrentHashMap](http://nivelle.me/2017/12/02/%E5%B8%B8%E8%A7%81%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84(6)-ConcurrentHashMap/))

- ([ArrayList](http://nivelle.me/2017/11/28/%E5%B8%B8%E8%A7%81%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84(3)-ArrayList/))

- ([LikedList](http://nivelle.me/2017/11/28/%E5%B8%B8%E8%A7%81%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84(4)-LinkedList/))

- ([hashSet](http://nivelle.me/2017/12/02/%E5%B8%B8%E8%A7%81%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84(5)-HashSet/))

- ([队列](http://nivelle.me/2017/12/02/%E9%98%9F%E5%88%97/))

- ([hashtable和hashMap](https://blog.csdn.net/tgxblue/article/details/8479147))

**线程池用过么？都有哪些参数？ 底层如何实现？**

#### **synchized和lock什么区别？ 底层细节**

- ([Synchronized](https://juejin.im/post/5b4eec7df265da0fa00a118f))

#### **threadLocal是什么？底层如何实现？鞋一个例子**

- ([threadLocal基本原理](http://nivelle.me/2017/07/01/ThreadLocal%E5%8E%9F%E7%90%86%E5%AD%A6%E4%B9%A0/))

#### **volitile的工作原理**

- ([volitile基本原理](https://www.cnblogs.com/dolphin0520/p/3920373.html))

**cas知道吗？如何实现**？

**四种写法**，**写一个单例模式**

#### **jvm内存模型？用过哪些垃圾回收器？说说**

- ([java内存模型](https://juejin.im/post/5b7d69e4e51d4538ca5730cb))

**线上频繁发生fullGC 如何处理？ CPU使用频率过高怎么办**？

**如何定位问题?如何解决？思路和方法**

**字节码有哪些**？

#### **Integer x= 5, int y=5 比较x==y有哪些步骤**

- ([基本数据类型拆箱装箱1](https://juejin.im/post/5ad158d2f265da2381560d83))

 - ([基本数据类型拆箱装箱2](https://juejin.im/post/5ad1e46951882555784e65d8))

#### **类加载机制？ 类加载器？分别加载哪些文件？手写一个**

- ([类加载机制](http://nivelle.me/2017/09/23/java%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6/))

**osgi?它是如何实现的**

**jvm优化？使用什么方法？达到什么效果**

**class.forName() 和 class.getClassLoader().loadClass()什么区别**

**spring 哪些机制？aop如何实现？IOC如何实现**

**cglib知道么？jdk动态代理？手写一个动态代理的例子**

#### **https**

- ([https原理](https://juejin.im/post/5a2ff29c6fb9a045132aac5a))

#### **git原理**

- ([git原理](https://juejin.im/post/5a65ac67f265da3e330473f7))

#### **子网掩码**

- ([子网掩码的原理和应用](https://blog.csdn.net/faker_wang/article/details/80747407))

#### 动态代理

- ([基于CGLIB](https://juejin.im/post/5b3e05caf265da0f652364ce))
- ([基于JDK](https://juejin.im/post/5b39dee0e51d4558cc35e3a5))

#### NIO

- ([javaNIO](https://juejin.im/post/5b21d775e51d4506b53ec412))
