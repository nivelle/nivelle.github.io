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
-([mysql索引章节](http://nivelle.me/2017/05/04/索引/))
-([mysql树结构](http://blog.jobbole.com/111680/))
-([mysql索引实现](https://www.cnblogs.com/zlcxbb/p/5757245.html))
  
2. mysql 有哪些存储引擎？有什么区别？ 
-([mysql索引类型比较](https://www.cnblogs.com/changna1314/p/6878900.html))
 
3. 设计高并发系统数据库层面应该怎么设计？ 数据库锁有哪些类型？如何实现

4. 数据库事物有哪些？

5. 如何设计可以动态扩容的分库分表方案？ 以及底层原理？常见的分库分表中间件？优缺点？ 如何让未分库分表的数据动态切换到分库分表的系统上？分库分表解决主键问题?
6.分布式事物？如何实现？TCC？ 网络出现问题，如何容错？

7.分布式寻址方式方式有哪些算法？ 一致性hash算法

8. redis 和 memcheched 什么区别？为什么单线程的redis比多线程的memched效率高？

9. redis主要数据类型？分别那种场景下使用？

10. redis的主从复制怎么实现的？redis集群模式是如何实现的？ redis的key是如何寻址的？

11. 使用redis如何设计分布式锁？ 使用zk可以吗？如何实现？两种效率更高？

12. redis持久化？各有什么优缺点？具体底层实现？
13. redis过期策略？lru ? 写下java版本的代码？

14. dubbo的实现过程？注册中心挂了可以继续通信么？dubbo常见配置有哪些

15. zk 原理? zk的应用？ paxos算法？

16. dubbo支持哪些序列化协议？hession?hession数据结构？ pb知道么？为啥PB的效率是最高的？

17. netty 可以干什么？ NIO，BIO ，AIO 都是什么？ 有什么区别

18.dubbo负载均衡策略和高可用策略有哪些？动态代理策略呢？

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

31. 45亿阿拉伯数据，如何进行去重复？如何找出最大的那个？

32. 二叉树和红黑树等常见数据结构

33. hashcode相等两个类一定相等吗？ equals 呢？ 相反呢？

34. 介绍下集合框架

35. hashmap和hashtable底层实现？ hashtable 和 concurrenthashtable呢？

36. hashmao 和treemap什么区别，数据结构

37. 线程池用过么？都有哪些参数？ 底层如何实现？
38. synchized和lock什么区别？ 底层细节
39. threadLocl是什么？底层如何实现？鞋一个例子
40. volitile的工作原理
41.cas知道吗？如何实现？
42.四种写法，写一个单例模式
43.jvm内存模型？用过哪些垃圾回收器？说说
44.线上频繁发生full GC 如何处理？ CPU使用频率过高怎么办？
45.如何定位问题》如何解决？思路和方法
46. 字节码有哪些？
47. Integer x= 5, int y=5 比较x==y有哪些步骤
48. 类加载机制？ 类加载器？分别加载哪些文件？手写一个
49.osgi?它是如何实现的
50.jvm优化？使用什么方法？达到什么效果
51. class.forName() 和 class.getClassLoader().loadClass()什么区别
52. spring 哪些机制？aop如何实现？IOC如何实现
53.cglib知道么？jdk动态代理？手写一个动态代理的例子
