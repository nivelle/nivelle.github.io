---

layout: post
title:  "springIOC之Application"
date:   2017-04-23 00:06:05
categories: 技术
tags: spring
excerpt: Application

---


* content
{:toc}

---

###  ApplicationContext

#####  统一资源加载策略
- Resource(资源抽象)和 ResourceLoader（资源加载策略）

  Resource接口可以根据资源的不同类型或者场合，给出对应的实现。Spring一些常用的实现类:
  
  - ByteArrayResource：
  
    将字节数组提供的数据作为资源封装，通过InputStrem访问资源，则会根据字节数组的数据，构造相应的ByteArrayInputStream并返回。
  - ClassPathResource: 
    
    从java应用程序的ClassPath中加载具体资源并进行封装，可以使用指定的类加载器（classLoader）或者给定的类进行资源的加载。 

  - FileSystemResource:
    
    对java.io.File类型的封装，可以以文件或者url的形式对该类型资源进行访问。

  - UrlResource:
  
    通过java.net.URL进行的具体资源查找定位实现类，内部委派URL进行具体的资源操作。

  - InputStreamResource
  
    经常以ByteArrayResource 代替。





#####  国际化信息支持


#####  容器内部事件发布

