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



1. 使用mysql索引都有哪些原则？索引有什么数据结构？ B+tree 和 B tree 什么区别 

- ([mysql索引章节](http://nivelle.me/2017/05/04/索引/))

- ([mysql树结构](http://blog.jobbole.com/111680/))

- ([mysql索引实现](https://www.cnblogs.com/zlcxbb/p/5757245.html))
- ([索引使用](https://juejin.im/post/5b14e0fd6fb9a01e8c5fc663))
  
2. mysql 有哪些存储引擎？有什么区别？ 

- ([mysql索引类型比较](https://www.cnblogs.com/changna1314/p/6878900.html)) 

- ([mysql索引类型以及特点](https://my.oschina.net/junn/blog/183341))

- ([mysqlAUTO_INCREMENT](https://blog.csdn.net/zm2714/article/details/8482211))
 
3. 设计高并发系统数据库层面应该怎么设计？ 数据库锁有哪些类型？如何实现

- ([高并发系统设计-数据层](https://blog.csdn.net/chenpeng19910926/article/details/51789934))

- ([mysqlAUTO_INCREMENT](https://blog.csdn.net/wangkehuai/article/details/46727203))
- ([mysql 高并发方案](https://blog.csdn.net/kidoo1012/article/details/54691561))

- ([架构设计 数据库方案](https://www.w3cschool.cn/architectroad/architectroad-arv823bf.html))


4. 数据库事物有哪些？

- ([数据库事务基础](http://nivelle.me/2017/06/05/%E4%BA%8B%E7%89%A9%E5%AD%A6%E4%B9%A0(%E4%B8%80)%E4%B9%8B%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5/))

- ([数据库事务使用](http://nivelle.me/2017/06/10/%E4%BA%8B%E5%8A%A1%E4%BD%BF%E7%94%A8/))

5. 如何设计可以动态扩容的分库分表方案？ 以及底层原理？常见的分库分表中间件？优缺点？ 如何让未分库分表的数据动态切换到分库分表的系统上？分库分表解决主键问题?
- ([分库分表方案](https://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=2650994413&idx=1&sn=24a01089ee47793b5d82381b04a34499&chksm=bdbf0ebe8ac887a8a75a0cd9226bb7e0427f1a77c43323d14dc6932c7dc77555531bce5dce0f&scene=0))


6. 分布式事物？如何实现？TCC？ 网络出现问题，如何容错？

- ([分布式事务](http://www.infoq.com/cn/articles/solution-of-distributed-system-transaction-consistency#))
- ([分布式事务阶段提交](http://int64.me/2016/%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%8B%E5%8A%A12PC%20&&%203PC.html))
- ([TCC](https://juejin.im/post/5a74f3bc6fb9a0633f0df127))
7. 分布式寻址方式方式有哪些算法？ 一致性hash算法

- ([一致性hash算法](http://www.zsythink.net/archives/1182))

8. redis 和 memcheched 什么区别？为什么单线程的redis比多线程的memched效率高？
- ([redis的特点和原理](https://juejin.im/post/5ad6e4066fb9a028d82c4b66))

9. redis主要数据类型？分别那种场景下使用？

10. redis的主从复制怎么实现的？redis集群模式是如何实现的？ redis的key是如何寻址的？

11. 使用redis如何设计分布式锁？ 使用zk可以吗？如何实现？两种效率更高？

12. redis持久化？各有什么优缺点？具体底层实现？

13. redis过期策略？lru ? 写下java版本的代码？

14. dubbo的实现过程？注册中心挂了可以继续通信么？dubbo常见配置有哪些

15. zk 原理? zk的应用？ paxos算法？

16. dubbo支持哪些序列化协议？hession?hession数据结构？ pb知道么？为啥PB的效率是最高的？
- ([dubbo原理](https://juejin.im/post/5b1203f2e51d450689495cea))
17. netty 可以干什么？ NIO，BIO ，AIO 都是什么？ 有什么区别

18. dubbo负载均衡策略和高可用策略有哪些？动态代理策略呢？

19. 为什么要进行系统拆分啊？ 拆分不用dubbo可以么？ dubbo和thirft什么区别？

20. 为什么使用消息队列？消息队列的优点和缺点？

21. 如何保证消息队列的高可用？如何保证消息不被重复消费？

22. kafka,activemq,rabbitmq,rocketmq 都有什么优点和缺点？

23. 如何自己设计一个消息队列，该如何进行架构设计？ 

24. 如何设计一个高并发高可用系统？

25. 如何限流？工程中怎么做的？说下具体实现？

26. 缓存如何使用？缓存使用不当带来什么问题？

27. 如何降级？如何进行系统拆分，如何进行数据库拆分？

28. 说下tcp/ip四层协议

29. http的工作流程？越细节越好。  http1.0 1.1 1.2有哪些区别

30. tcp 三次握手，四层分手的工作流程〉为什么不是其他次数？

- ([计算机网络面试常见](https://www.jianshu.com/p/d0dd47afabad))

31. 45亿阿拉伯数据，如何进行去重复？如何找出最大的那个？

32. 二叉树和红黑树等常见数据结构

33. hashcode相等两个类一定相等吗？ equals 呢？ 相反呢？

34. 介绍下集合框架

35. hashmap和hashtable底层实现？ hashtable 和 concurrenthashtable呢？

36. hashmao 和treemap什么区别，数据结构

37. 线程池用过么？都有哪些参数？ 底层如何实现？

38. synchized和lock什么区别？ 底层细节

- ([Synchronized](https://juejin.im/post/5b4eec7df265da0fa00a118f))

39. threadLocal是什么？底层如何实现？鞋一个例子

- ([threadLocal基本原理](http://nivelle.me/2017/07/01/ThreadLocal%E5%8E%9F%E7%90%86%E5%AD%A6%E4%B9%A0/))

40. volitile的工作原理

- ([volitile基本原理](https://www.cnblogs.com/dolphin0520/p/3920373.html))

41.cas知道吗？如何实现？

42.四种写法，写一个单例模式

43.jvm内存模型？用过哪些垃圾回收器？说说

44.线上频繁发生full GC 如何处理？ CPU使用频率过高怎么办？

45.如何定位问题?如何解决？思路和方法

46.字节码有哪些？

47. Integer x= 5, int y=5 比较x==y有哪些步骤

48. 类加载机制？ 类加载器？分别加载哪些文件？手写一个

49.osgi?它是如何实现的

50.jvm优化？使用什么方法？达到什么效果

51.class.forName() 和 class.getClassLoader().loadClass()什么区别

52. spring 哪些机制？aop如何实现？IOC如何实现

53.cglib知道么？jdk动态代理？手写一个动态代理的例子

54. https

- ([https原理](https://juejin.im/post/5a2ff29c6fb9a045132aac5a))

55. git原理

- ([git原理](https://juejin.im/post/5a65ac67f265da3e330473f7))

56. 子网掩码

- ([子网掩码的原理和应用](https://blog.csdn.net/faker_wang/article/details/80747407))
